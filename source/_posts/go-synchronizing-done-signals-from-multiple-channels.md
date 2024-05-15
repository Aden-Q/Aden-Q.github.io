---
title: 'Go: Synchronizing Done Signals from Multiple Channels'
tags:
  - Go
date: 2023-12-11 21:00:00
categories:
---

## Problem Statement

In certain scenarios, a single task can be distributed among multiple workers, and the requirement is to await the completion of the task by the fastest worker. This technique is also called "replicated requests".

A tangible example is a web server, wherein the server typically maintains a worker (goroutine/thread/process) pool to concurrently handle incoming requests. Some workers may become overwhelmed due to network I/O, disk I/O, etc., while others remain idle.

Another example is in a web server cluster setup, where horizontal scaling is employed to deploy multiple servers for the same web service to ensure high availability. In such a setup, certain servers may become overwhelmed due to high load, while others remain idle.

In both scenarios, we can replicate the request to multiple workers and return the result immediately when any one of the workers finishes the task.

Without a smart load balancing policy, users may experience delays in response latency in both situations, leading to a high round-trip time (RTT). Instead of assigning a task to a single worker and blocking while awaiting results, optimizing server performance sometimes requires assigning the same task to multiple workers and waiting for a response from the fastest worker. This approach can be especially beneficial in high-availability setups, where we can deploy a reverse-proxy server in front of the server clusters and distribute the same request to a server set while only taking the fastest response.

This issue can be formulated as synchronizing multiple goroutines in Go.

## Using the or-channel concurrency pattern

This approach uses the channel synchronization primitive derived from Communicating Sequential Processes (CSP). The *or-channel* is a commonly used concurrency pattern in Go where we want to select data from multiple 'done' channels, and we are interested in the first piece of data that becomes available from any of the channels. This pattern is similar to the logical `OR` operation.

### When the number of 'done' channels is unknown beforehand

This situation is quite simple; we can write a function to combine multiple 'done' channels into a single 'done' channel. As an example, consider receiving from two 'done' channels:

```Go
func or(ch1, ch2 <-chan interface{}) <-chan interface{} {
  orDone := make(chan interface{})

  go func() {
    defer close(orDone)

    select {
    case <-ch1:
    case <-ch2:
    }
  }()

  return orDone
}
```

The `or` function combines two 'done' channels into a single 'done' channel and returns it. Here, we use interface channels so that we can accept all types of data from workers. In real scenarios, we should use concrete types or type assertion since Go interfaces are not type-safe. Also, since we only await the fastest goroutine to give us a result, make sure to prevent goroutine leaks.

Full Code snippet for testing:

```Go
// main.go
package main

import (
  "fmt"
  "time"
)

func or(ch1, ch2 <-chan interface{}) <-chan interface{} {
  orDone := make(chan interface{})

  go func() {
    defer close(orDone)

    select {
    case <-ch1:
    case <-ch2:
    }
  }()

  return orDone
}

func main() {
  launchWorker := func(after time.Duration) <-chan interface{} {
    // Instantiate a done channel
    ch := make(chan interface{})

    go func() {
      time.Sleep(after)
      ch <- true
    }()

    return ch
  }

  worker1 := launchWorker(1 * time.Second)
  worker2 := launchWorker(2 * time.Second)

  // Use or function to wait for the first worker to finish
  start := time.Now()
  <-or(worker1, worker2)

  fmt.Println("Received a done signal, exiting after", time.Since(start))
}
```

Results:

```bash
➜  demo go run main.go
Received a done signal, exiting after 1.000374497s
```

### When the number of 'done' channels is known

When the number of workers is unknown, this is a more common situation, and when we need a generic variadic function to synchronize multiple goroutines in this case.

We updated the `or` function written above to be a variadic function:

```Go
func or(channels ...<-chan interface{}) <-chan interface{} {
  orDone := make(chan interface{})
  // Use a buffered channel to accept the first value from any of the input channels
  syncChan := make(chan interface{}, 1)

  // Function to monitor input channels
  monitor := func(ch <-chan interface{}) {
    syncChan <- <-ch
  }

  // Start goroutines to monitor each channel
  for _, ch := range channels {
    go monitor(ch)
  }

  // Wait for any of the goroutines to close orDone
  go func() {
    // Unblocked when any of the workers sends a done signal
    <-syncChan
    close(orDone)
  }()

  return orDone
}
```

Here, we launch a monitor goroutine for each worker channel and introduce a buffered channel to accept a result from any of the input channels. The `orDone` channel will be closed if any of the workers finish their work.

Full Code snippet for testing:

```Go
// main.go
package main

import (
  "fmt"
  "time"
)

func or(channels ...<-chan interface{}) <-chan interface{} {
  orDone := make(chan interface{})
  // Use a buffered channel to accept the first value from any of the input channels
  syncChan := make(chan interface{}, 1)

  // Function to monitor input channels
  monitor := func(ch <-chan interface{}) {
    syncChan <- <-ch
  }

  // Start goroutines to monitor each channel
  for _, ch := range channels {
    go monitor(ch)
  }

  // Wait for any of the goroutines to close orDone
  go func() {
    // Unblocked when any of the workers sends a done signal
    <-syncChan
    close(orDone)
  }()

  return orDone
}

func main() {
  launchWorker := func(after time.Duration) <-chan interface{} {
    // Instantiate a done channel
    ch := make(chan interface{})

    go func() {
      time.Sleep(after)
      ch <- true
    }()

    return ch
  }

  // Use or function to wait for the first worker to finish
  start := time.Now()
  <-or(
    launchWorker(1*time.Second),
    launchWorker(2*time.Second),
    launchWorker(3*time.Second),
    launchWorker(5*time.Second),
    launchWorker(10*time.Second),
  )

  fmt.Println("Received a done signal, exiting after", time.Since(start))
}
```

Results:

```bash
➜  demo go run main.go
Received a done signal, exiting after 1.000385271s
```

## Using context

While the *or-channel*, as mentioned above, can be used to receive a 'done' signal from the workers, the `context` package can be used to gracefully shut down all working goroutines if their tasks are not needed anymore. However, since goroutines are non-preemptive, if goroutines for workers are involved in some blocking call (something like `time.Sleep`), then there's no way to terminate them when they are blocked.

Here is an example to shut down all child goroutines from their parent once the work is done:

```Go
// main.go
package main

import (
  "context"
  "fmt"
  "sync"
  "time"
)

func main() {
  var wg sync.WaitGroup
  ctx, cancel := context.WithCancel(context.Background())
  // cancel context after 2 seconds to simulate work done
  time.AfterFunc(2*time.Second, func() {
    cancel()
  })

  launchWorker := func(context context.Context) {
    defer wg.Done()
    for {
      select {
      case <-context.Done():
        return
      case <-time.After(1 * time.Second):
      }

      // worker running
    }
  }

  numWorkers := 10
  for i := 0; i < numWorkers; i++ {
    wg.Add(1)
    go launchWorker(ctx)
  }

  start := time.Now()
  wg.Wait()
  fmt.Println("all workers done after", time.Since(start).Seconds(), "seconds")
}
```

Results:

```bash
➜  demo go run main.go
all workers done after 2.001033811 seconds
```

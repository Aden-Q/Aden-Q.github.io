---
title: MIT 6.NULL - Course Notes
date: 2023-11-14 01:49:35
tags:
categories:
  - MOOC
---

This introductory course aims to enhance the proficiency of CS students with their tools. It covers a broad range of topics without delving too deeply into each, including command-line usage, shell programming, editors, version control, debugging, profiling, build systems, testing, etc. While some of these concepts may not seem immediately useful during a student's campus life, they prove to be highly valuable in the real industry.

While I won't cover every single detail of the course, I'll highlight some key takeaways that I personally find very helpful.

---

**Content**

- [Shell Tools and Scripting](#shell-tools-and-scripting)
  - [Special Variables for the Bash Shell](#special-variables-for-the-bash-shell)
  - [Finding Files](#finding-files)
  - [Search Context](#search-context)
  - [Search in History and History-Based Autosuggestions](#search-in-history-and-history-based-autosuggestions)
  - [Directory Navigation](#directory-navigation)
  - [File Editing](#file-editing)
- [Editors](#editors)
- [Command-line Environment](#command-line-environment)
  - [Job Control](#job-control)
  - [Terminal Multiplexers](#terminal-multiplexers)
  - [Alias](#alias)
  - [Dotfiles](#dotfiles)
  - [Remote Development with SSH](#remote-development-with-ssh)
- [Logging](#logging)
- [Profiling](#profiling)
  - [Resource Monitoring Tools](#resource-monitoring-tools)
- [Metaprogramming](#metaprogramming)
  - [Build Systems](#build-systems)
  - [Dependency and Semantic Versioning](#dependency-and-semantic-versioning)
  - [Continuous integration (CI) systems](#continuous-integration-ci-systems)
- [Security and Cryptography](#security-and-cryptography)
  - [Entropy](#entropy)
  - [Hash Functions](#hash-functions)
  - [Symmetric Cryptography](#symmetric-cryptography)
  - [Asymmetric Cryptography](#asymmetric-cryptography)
- [Potpourri](#potpourri)
  - [Daemons](#daemons)
  - [FUSE](#fuse)
  - [Command Line Arguments](#command-line-arguments)
  - [Hammerspoon (desltop automation on macOS)](#hammerspoon-desltop-automation-on-macos)
  - [Booting + Live USBs](#booting--live-usbs)

## Shell Tools and Scripting

### Special Variables for the Bash Shell

- `$0`: Name of the script/program
- `$1` to `$9`: Arguments to the script
- `$@`: All the arguments
- `$#`: Number of arguments
- `$?`: Return code of the previous commad
- `$$`: PID for the current script
- `!!`: Entire last command
- `$_`: Last argument from the last command

### Finding Files

+ `find`: a program coming along with all Linux distrbutions
+ `fd`: a simple, fast, and user-friendly alternatively to `find`
+ `locate`: uses a database to index and search

### Search Context
+ `grep`: provided by most Linux distributions
+ `ack`: an alternative to `grep`
+ `ag`: an alternative to `grep`
+ `rg`: ripgrep, a fast alternative to `grep`

### Search in History and History-Based Autosuggestions

`fzf` is a general-purpose fuzzy finder that can be used to search from any input stream. As an example, we can use `fzf` to search the output from `history` interactively by running:

```bash
$ history | fzf
```

Another very cool history-related trick is **history-based autosuggestions**. This feature dynamically autocompletes the current shell command with the most recent command in the history sharing a common prefix. It can be enabled in **zsh** with a plugin.

![](https://raw.githubusercontent.com/Aden-Q/blogImages/main/img/20231114010908.png)

### Directory Navigation

Tools like `fasd` and `autojump` can be used to find frequent and recent files/directories.

More complex tools like `tree`, `broot`, `nn`, `ranger` can be used to get an overview of a directory structure.

### File Editing

`sed`, `awk` are useful tools to edit files/input streams. Both have their own programming languages and can be used along with **regex**, a powerful tool for pattern matching. Even if we do not use shell, it's crucial to be familiar with regex for a developer.

**Misc**

The `xargs` command can be used to execute a command using STDIN as arguments. We might need to use this tool sometimes because some commands take both STDIN (i.e. any input stream) and command line arguments, but some commands like `tar` and `rm` only take input from arguments. As an example:

```bash
$ ls | xargs rm
```

This command will delete the files in the current directory. Of course there is a better way to do the same thing. This is just an example for illustration purposes.

## Editors

I personally use **VSCode** for both my side projects and work. While it comes with a vibrant remote development toolset, including features like SSH, tunneling, remote containers, and VSCode server, it may not cover all circumstances. There are situations where a command-line editor like **Vim** is still beneficial.

## Command-line Environment

### Job Control

**Killing a Process**

The shell can use an inter-process communication mechanism called a *signal*. When a process receives a signal, it stops its execution, deals with the signal, and potentially changes the flow of execution based on the information that the signal delivered. For this reason, signal is also called *software interrupt*.

+ `Ctrl-C`: sends a `SIGINT` signal to a process to stop its execution
+ `Ctrl-\`: sends a `SIGQUIT` signal to a process to quit the process
+ `kill -TERM <PID>`: sends a `SIGTERM` signal to terminate a process gracefully

**Pausing and Backgrounding Processes**

Besides killing a process, we can also pause and resume a process.

+ `Ctrl-Z`: sends a `SIGSTOP` signal to a process to pause it
+ `fg`: continues the paused job in the foreground
+ `bg`: continues the paused job in the background

The `job` command lists all unfinished jobs associated with the current terminal session. The `pgrep` command can be used to find the associated process id of each.

The `&` suffix in a command will run the command in the background.

To background an already running program, do `Ctrl-Z` followed by `bg`.

Note that processes backgrounded by these methods are child processes of the current terminal and will terminate if the terminal is closed (with a `SIGUP` signal). To prevent this, use `nohup`, which is a wrapper that ignores `SIGUP`.

### Terminal Multiplexers

`tmux` is a powerful tool used for terminal multiplexing. It can maintain persistent sessions and connections to remote machines over SSH.

`tmux` has three core concepts:
+ Sessions: a session is an independent workspace with one or more windows
  - `tmux` starts a new session
  - `tmux new -s NAME` starts it with that name
  - `tmux ls` lists the current sessions
  - Within `tmux` typing `<C-b> d` detaches the current session
  - `tmux a` attaches the last session. You can use -t flag to specify which session to attach to
- Windows: equivalent to tabs in editors or browsers
    - `<C-b> c` creates a new window. To close it you can just terminate the shells doing `<C-d>`
    - `<C-b> N` go to the N th window. Note windows are numbered
    - `<C-b> p` goes to the previous window
    - `<C-b> n` goes to the next window
    - `<C-b> ,` renames the current window
    - `<C-b> w` lists current windows
- Panes: like vim splits, panes let you have multiple shells in the same visual display
    - `<C-b> "` splits the current pane horizontally
    - `<C-b> %` splits the current pane vertically
    - `<C-b> <arrow key>` moves to the pane in the specified direction
    - `<C-b> z` toggles zoom for the current pane
    - `<C-b> [` starts scrollback. You can then press `<space>` to start a selection and <enter> to copy that selection
    - `<C-b> <space>` cycles through pane arrangements

Note that we can make those shortcuts even "shorter" or more intuitive by customizing them in `.tmux.conf`. Please check [my config](https://github.com/Aden-Q/dotfiles/blob/main/tmux.conf) as an example.

### Alias

Most shells support aliasing. A shell alias is a short form for another command that your shell will replace automatically for you. For instance:

```bash
$ alias ga="git add"
$ alias gc="git commit"
$ alias gp="git push"
```

To make the alias persistent, we need to add them to the shell configuration.

### Dotfiles
Many programs are configured using plain-text files know as *dotfiles*. Some examples are:

+ `bash`: `~/.bashrc`, `~/.bash_profile`
+ `git`: `~/.gitconfig`
+ `vim`: `~/.vimrc`, `~/.vim` folder
+ `ssh`: `~/.ssh/confg`
+ `tmux`: `~/.tmux.conf`
+ `.netrc`: a magic file 

Organizing dotfiles can help us easily migrate to a new machine/environment. A typical way to organize them is using a GitHub repo. [Here](https://github.com/Aden-Q/dotfiles) is the repo holding all my dotfiles.

### Remote Development with SSH

It is very common for developers to use remote machines, and a powerful tool for this purpose is the **Secure Shell** (**SSH**). An alternative to `ssh` is `mosh`, developed by MIT, which supports long-lived connections.

**Copying files over SSH**

There are a few ways to copy files from local to a remote machine:

+ `ssh+tee`: `tee` writes the output from STDIN to a file and returns the file handler. As an example: `cat localfile | ssh remote_server tee serverfile`
+ `scp`: a secure copy command used to copy large amounts of files/directories. The syntax is `scp path/to/local_file remote_host:path/to/remote_file`
+ `rsync`: a program improves upon `scp` to skip duplicate

**Port Forwarding**

Port forwarding is a useful technique, especially in web development or when dealing with a service listening on a remote host's port that is not directly accessible through the network or the internet.

There are two types of port forwarding, as the image below shows:

![](https://raw.githubusercontent.com/Aden-Q/blogImages/main/img/port_forwarding.png)

We can use SSH with port forwarding to map a remote host's port to a local one. As an example:

```bash
$ ssh -L local_port:localhost:remote_port user@remote_host
```

This command forwards traffic from the local port to the specified remote port on the remote host.

## Logging

Logging is better than regular print statements for debugging for several reasons:

+ We can log to files, socket, or even remote machines instead of STDOUT.
+ Logging supports severity levels (such as INFO, DEBUG, WARN, ERROR, &c) that allow us to filter the output accordingly.

In UNIX systems, usually programs write their logs under `/var/log`. There is also a **system log**.

`systemd` is a system daemon that controls many things such as which services are enabled and running. `systemd` places logs under `/var/log/journal` in specialized format that can be parsed and displayed by the `journalctl` command.

The `dmesg` command can be used to access the kernel log.

Another useful tool to filter and display logs is `log show`.

## Profiling

Profilers and monitoring tools can help us understand which parts of our program are taking up most of the time/resources and becoming bottlenecks for performance. This allows us to focus on optimizing those specific parts.

Most commonly used profilers are CPU profilers and memory profilers.

There is also something called *event profiling*. The `perf` command can report system events related to a program. It can easily report things including cache locality, high amounts of page faults or livelocks.

A [Flame Graph](http://www.brendangregg.com/flamegraphs.html) can be used to show profiling information:

![](https://raw.githubusercontent.com/Aden-Q/blogImages/main/img/20231114114900.png)

### Resource Monitoring Tools

There are several tools available to monitor various system resources. A few common ones are listed below:

+ **General monitoring**: `top`, `htop`, `glances`, `dstat`
+ **I/O operations**: `iotop`
+ **Disk usage**: `df`, `du`, `ncdu`
+ **Memory usage**: `free`
+ **Open files**: `lsof`` lists file information about files opened by processes. It can be quite useful for checking which process has opened a specific file
+ **Network connections and config**: `ss` lets you monitor incoming and outgoing network packet statistics as well as network interface statistics. `ip` can be used to display routing
+ **Network usage**: `nethogs` and `iftop`

There is also a tool `hyperfine` that allows quickly benchmark commands.

## Metaprogramming

### Build Systems

Build systems usually share some common characteristics:

- dependencies
- targets
- rules

`make` is one of the most commonly used build tools.

### Dependency and Semantic Versioning

Recently during my work, I also encountered the concetps of [semantic vesioning](https://www.conventionalcommits.org/en/v1.0.0/) and [conventional commits](https://www.conventionalcommits.org/en/v1.0.0/). Those two concepts are important in the sense to allow automatic version bump, changelogs, and a safe release cycle for programs.

Sematic versioning follows the form: `major.minor.patch`. The core rules are:

- If a new release does not change the API, increase the patch version.
- If you add to your API in a backwards-compatible way, increase the minor version.
- If you change the API in a non-backwards-compatible way, increase the major version.

Semantic versioning is crucial in the lifecycle of project development because:

+ It is safe to use the latest release within the same major version as our project's original dependency. (Think about the backwards compatibility rule). As an example, if the project depends on a library at version `1.3.7`, then it should be safe t build it with `1.3.8`, `1.6.1`, or even `1.3.0`. However, `2.2.4` may not work.

There is also a notion of *lock files* in dependency management. A lock file lists all the versions the project currently depends on. Using the lock file has the following benefits:

- Avoiding unnecessary recompiles
- Having reproducible builds
- Not automatically updating to the latest version

### Continuous integration (CI) systems

A continuous integration system comprises a series of workflows triggered by specific events, automating a range of tasks such as testing, building, versioning, and deploying. This streamlined automation not only enhances efficiency but also ensures the reliability and consistency of software development processes.

Some examples of CI systems:

- Travis CI
- Azure Pipelines
- GitHub Actions
- CircleCI

Different types of tests:

- Unit test: a “micro-test” that tests a specific feature in isolation, it can be a single function
- Integration test: a “macro-test” that runs a larger part of the system to check that different feature of components work together
- Regression test: a test that implements a particular pattern that previously caused a bug to ensure that the bug does not resurface
- Mocking: to replace a function, module, or type with a fake implementation to avoid testing unrelated functionality. For example, we can mock the network, or disk.

## Security and Cryptography

### Entropy

Entropy is a meansure of randomness.

### Hash Functions

We mainly focus on [cryptographic hash functions](https://en.wikipedia.org/wiki/Cryptographic_hash_function). An example of a hash function is [SHA1](https://en.wikipedia.org/wiki/SHA-1).

A hash function has the following properties:

- Deterministic: the same input always generates the same output
- Non-invertible: it is hard to find an input `m` such that `hash(m) = h` for some desired output `h`
- Target collision resistant: given an input `m_1`, it’s hard to find a different input `m_2` such that `hash(m_1) = hash(m_2)`
- Collision resistant: it’s hard to find two inputs `m_1` and `m_2` such that `hash(m_1) = hash(m_2)`

**Applications**:
+ Git
+ A short summary of the contents of a file
+ [Commitment schemes](https://en.wikipedia.org/wiki/Commitment_scheme)

### Symmetric Cryptography

```
keygen() -> key  (this function is randomized)

encrypt(plaintext: array<byte>, key) -> array<byte>  (the ciphertext)
decrypt(ciphertext: array<byte>, key) -> array<byte>  (the plaintext)
```

An example is [AES](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard).

### Asymmetric Cryptography

```
keygen() -> (public key, private key)  (this function is randomized)

encrypt(plaintext: array<byte>, public key) -> array<byte>  (the ciphertext)
decrypt(ciphertext: array<byte>, private key) -> array<byte>  (the plaintext)

sign(message: array<byte>, private key) -> array<byte>  (the signature)
verify(message: array<byte>, signature: array<byte>, public key) -> bool  (whether or not the signature is valid)
```

An example is [RSA](https://en.wikipedia.org/wiki/RSA_(cryptosystem)).

**Applications:**

+ PGP email encryption
+ Private messaging: Telegram
+ Digital signature

## Potpourri

### Daemons

In Linux, `systemd` is the most common solution for running and setting up daemon processes. We can run `systemctl status` to list the current running daemons, in a tree structure. `systemd` can be interacted with the `systemctl` command in order to `enable`, `disable`, `start`, `stop`, `restart` or check the `status` of services.

### FUSE

FUSE (Filesystem in User Space) allows filesystems to be implemented by a user program. FUSE lets users run user space code for filesystem calls and then bridges the necessary calls to the kernel interfaces.

**Applications:**

- [sshfs](https://github.com/libfuse/sshfs) - Open locally remote files/folder through an SSH connection.
- [rclone](https://rclone.org/commands/rclone_mount/) - Mount cloud storage services like Dropbox, Google Drive, Amazon S3 or Google Cloud Storage and open data locally.
- [gocryptfs](https://nuetzlich.net/gocryptfs/) - Encrypted overlay system. Files are stored encrypted but once the FS is mounted they appear as plaintext in the mountpoint.
- [kbfs](https://keybase.io/docs/kbfs) - Distributed filesystem with end-to-end encryption. You can have private, shared and public folders.
- [borgbackup](https://borgbackup.readthedocs.io/en/stable/usage/mount.html) - Mount your deduplicated, compressed and encrypted backups for ease of browsing.

### Command Line Arguments

- A `--help` flag can be used to display brief usage instructions for the tool.
- A `--version` or `-V` flag can be used to print the program’s version
- A `--verbose` or `-v` flag produces more verbose output. The flag can be included multiple times  (`-vvv`) to get more verbose output. Similarly, a `--quiet` flag can be used to only print something on error.
- In many tools, `-` in place of a file name means “standard input” or “standard output”, depending on the argument.
- The special argument `--` makes a program *stop* processing flags and options (things starting with `-`) in what follows, letting you pass things that look like flags without them being interpreted as such: `rm -- -r` or `ssh machine --for-ssh -- foo --for-foo`.

### Hammerspoon (desltop automation on macOS)

[Hammerspoon](https://www.hammerspoon.org/) is a desktop automation framework for macOS. It lets us to write Lua scripts that hook into OS functionality, allowing us to interact with the keyboard/mouse, windows, displays, filesystem, and much more.

**Applications:**

- Bind hotkeys to move windows to specific locations
- Create a menu bar button that automatically lays out windows in a specific layout
- Mute your speaker when you arrive in lab (by detecting the WiFi network)
- Show you a warning if you’ve accidentally taken your friend’s power supply

### Booting + Live USBs

We can boot a OS from a live USB, by using BIOS/UEFI to initialize the system.

Live USBs are useful for many purposes. For example, when we break the existing operating system so it can no longer boot, we can use a live USB to recover data or fix the operating system.

[UNetbootin](https://unetbootin.github.io/) is a powerful tool to help create live USBs.

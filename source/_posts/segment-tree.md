---
title: Segment Tree
tags:
  - Python
  - Algorithm
date: 2024-03-31 22:00:00
categories:
---

**Content**

- [Introduction](#introduction)
- [Implementation](#implementation)
  - [Tree (Class)](#tree-class)
    - [Build](#build)
    - [Range Query](#range-query)
    - [Update](#update)
    - [Example run](#example-run)
    - [Pros and Cons](#pros-and-cons)
  - [Implicit Tree (Array)](#implicit-tree-array)
    - [Build](#build-1)
    - [Range Query](#range-query-1)
    - [Update](#update-1)
    - [Example run](#example-run-1)
    - [Pros and Cons](#pros-and-cons-1)
- [References](#references)

## Introduction

The segment tree is an efficient binary tree data structure where each node represents an interval, allowing range queries and dynamic updates to run in logarithmic time.

## Implementation

For convenience, we consider a bounded integer array as the input and perform range sum queries on it, utilizing only a 1D discrete segment tree.

### Tree (Class)

Tree definition:

```python
class SegTreeNode:
    def __init__(self, lo, hi, val=0, left=None, right=None):
        # left endpoint of the segment
        self.lo = lo
        # right endpoint of the segment
        self.hi = hi
        # the value of this segment
        self.val =val
        # left child
        self.left = left
        # right child
        self.right = right
```

`merge` function:

```python
# merge will only be called on the parent node
# every parent node has exactly 2 children
def merge(node1: SegTreeNode, node2: SegTreeNode) -> int:
    return node1.val + node2.val
```

#### Build

The `build` function runs in $O(N)$ time, where $N$ is the maximum range.

```python
# arr: input array
# lo: left endpoint of the segment
# hi: right endpoin of the segment
# example call: build(arr, 0, n-1) where n is the length of the input array
def build(arr: list[int], lo: int, hi: int) -> Optional[SegTreeNode]:
    if lo == hi:
        return SegTreeNode(lo, hi, arr[lo])
    
    root = SegTreeNode(lo, hi)
    mid = lo + (hi - lo) // 2
    root.left = build(arr, lo, mid)
    root.right = build(arr, mid+1, hi)
    # merge operation to aggregate the values from children
    root.val = merge(root.left, root.right)

    return root
```

#### Range Query

The `rangeQuery` function runs in $O(log N + k)$ time, where $N$ is the maximum range and $k$ is the number of retrieved segments. As $k$ is bonunded by $4log N$, the overall time complexity is $O(log N)$.

```python
# root: the root of the segment tree
# left: range query left endpoint (inclusive)
# right: range query right endpoint (inclusive)
def rangeQuery(root: SegTreeNode, left: int, right: int) -> int:
    # query out of the segment range
    if left > root.hi or right < root.lo:
        return 0

    # query exactly matches the segment range
    if root.lo == left and root.hi == right:
        return root.val

    mid = root.lo + (root.hi - root.lo) // 2

    return rangeQuery(root.left, left, min(right, mid)) + rangeQuery(root.right, max(mid+1, left), right)
```

#### Update

The `update` functions runs in $O(log N)$ time, where $N$ is the maximum range.

```python
# root: the root of the segment tree
# index: the index of the element in the original array to be updated
# val: the value of the element to be update to
def update(root: SegTreeNode, index: int, val: int) -> None:
    if root.lo == root.hi == index:
        root.val = val
        return
    
    mid = root.lo + (root.hi - root.lo) // 2
    if index <= mid:
        update(root.left, index, val)
    else:
        update(root.right, index, val)

    root.val = merge(root.left, root.right)

    return
```

#### Example run

```python
print("Segment Tree Implementation with class")
arr = [1, 3, 5, 7, 9, 11]
print("Input Array: ", arr)
root = build(arr, 0, len(arr)-1)
print("Range query [1, 3]: ", rangeQuery(root, 1, 3))
print("Updating arr[2] from 5 to 7")
update(root, 2, 7)
print("New range query [1, 3]: ", rangeQuery(root, 1, 3))
```

```bash
Segment Tree Implementation
Input Array:  [1, 3, 5, 7, 9, 11]
Range query [1, 3]:  15
Updating arr[2] from 5 to 7
New range query [1, 3]:  17
```

#### Pros and Cons

**Pros:**

+ Implementing a segment tree with a tree class is much easier than the array approach (mentioned below).
+ No padding is needed to make it a perfect binary tree; we only allocate the necessary tree nodes.

**Cons:**

+ Memory inefficiency: Additional storage is required for child pointers and discrete storage. When traversing using pointers, there is a lot of random access.

### Implicit Tree (Array)

This is a memory-efficient version of the segment tree implementation. We use 1-index for segment tree nodes. For a parent node with index `treeIndex`, its left child has index `2*treeIndex` and its right child has index `2*treeIndex + 1`.

We will operate on a global array representing the segment tree named `tree`.

Input: `arr`

Tree definition:

```python
tree = [0] * (4 * len(arr))
```

`merge` function:

```python
# merge will only be called on the parent node
# every parent node has exactly 2 children
def merge(treeIndex1: int, treeIndex2: int) -> int:
    return tree[treeIndex1] + tree[treeIndex2]
```

#### Build

```python
# arr: input array
# treeIndex: index into the segment tree
# lo: left endpoint of the segment
# hi: right endpoint of the segment
def build(arr: list[int], treeIndex: int, lo: int, hi: int) -> None:
    if lo == hi:
        tree[treeIndex] = arr[lo]
        return

    mid = mid = lo + (hi - lo) // 2
    build(arr, 2*treeIndex, lo, mid)
    build(arr, 2*treeIndex + 1, mid+1, hi)
    tree[treeIndex] = merge(2*treeIndex, 2*treeIndex + 1)
    
    return
```

#### Range Query

```python
# treeIndex: index into the segment tree
# lo: left endpoint of the segment
# hi: right endpoint of the segment
# left: query left endpoint
# right: query right endpoint
def rangeQuery(treeIndex, lo, hi, left, right) -> int:
    # when the query range does not overlap with the current segment
    if left > hi or right < lo:
        return 0

    # query exactly matches the current segment
    if left == lo and right == hi:
        return tree[treeIndex]

    mid = lo + (hi - lo) // 2

    return rangeQuery(2 * treeIndex, lo, mid, left, min(right, mid)) + rangeQuery(2 * treeIndex + 1, mid+1, hi, max(mid+1, left), right)
```

#### Update

```python
# treeIndex: index into the segment tree
# lo: left endpoint of the segment
# hi: right endpoint of the segment
# arrIndex: index of the element to be updated in the input array
# val: the new value
def update(treeIndex: int, lo: int, hi: int, arrIndex: int, val: int) -> None:
    if lo == hi:
        tree[treeIndex] = val
        return

    mid = (lo + hi) // 2
    if arrIndex > mid:
        update(2 * treeIndex + 1, mid+1, hi, arrIndex, val)
    else:
        update(2 * treeIndex, lo, mid, arrIndex, val)

    tree[treeIndex] = merge(2 * treeIndex, 2 * treeIndex + 1)
```

#### Example run

```python
print("Segment Tree Implementation with array")
arr = [1, 3, 5, 7, 9, 11]
tree = [0] * (4 * len(arr))
n = len(arr)
print("Input Array: ", arr)
build(arr, 1, 0, n-1)
print("Range query [1, 3]: ", rangeQuery(1, 0, n-1, 1, 3))
print("Updating arr[2] from 5 to 7")
update(1, 0, n-1, 2, 7)
print("New range query [1, 3]: ", rangeQuery(1, 0, n-1, 1, 3))
```

```bash
Segment Tree Implementation with array
Input Array:  [1, 3, 5, 7, 9, 11]
Range query [1, 3]:  15
Updating arr[2] from 5 to 7
New range query [1, 3]:  17
```

#### Pros and Cons

**Pros:**

+ Memory efficiency: Arrays provide contiguous memory allocation, enabling faster access compared to pointer dereference.

**Cons:**

+ Padding required: The segment tree needs to be padded with zeros to ensure it forms a perfect binary tree, resulting in a size four times that of the input array.

+ Additional arguments needed: Extra arguments are necessary for functions such as build, rangeQuery, and update to determine the left and right boundaries of the current segment since the array itself lacks information about the segment range. Although it's possible to encapsulate these ranges within private functions, the implementation remains more complex compared to the previous approach.

## References

+ https://cp-algorithms.com/data_structures/segment_tree.html
+ https://leetcode.com/articles/a-recursive-approach-to-segment-trees-range-sum-queries-lazy-propagation/
+ https://www.geeksforgeeks.org/segment-tree-data-structure/
+ https://www.youtube.com/watch?v=rYBtViWXYeI

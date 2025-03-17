---
title: 'Go Slice: Tricks and Internals'
tags:
  - Go
date: 2025-03-10 22:00:00
categirues
---
Internals of Go slice

---


**Content**



## Slice

Note, it's based on Go 1.24. Different versions may vary on implementations.

https://github.com/golang/go/blob/master/src/runtime/slice.go


unsafe pointer

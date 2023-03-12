---
title: "Runtime Explained"
date: 2023-01-03T16:13:29+08:00
---

## RUNTIME

### Generally

* Compile time error
* Link time
* Runtime error

|| Compile Time Error | Runtime Error |
|-|-|-|
| Time | Earlier | Later |
| Error | Grammar Error, Semantic Error | Memory Error, Math Error|
|  | Easy | Hard|

### Specifically

* Runtime Library (stdio.h)
* Runtime Environment
  * No Runtime: C/C++, Rust
  * Light Runtime: Golang
  * Heavy Runtime: Java(JVM), Python(CPython), C#(.NET Runtime)
* JRE = JVM + Library

|| No Runtime | Runtime |
|-|-|-|
| Memory Mgmt | Mannually | Automatically |
| Thread Model | Rely on OS | Rely on Runtime |
| System Interaction | Directly | Indirectly(rely on Runtime) |
| Efficiency | High | Low(High on JIT) |
|| Bare Metal | ------- |

## Reference

[Discussion on RUNTIME - bilibili](https://www.bilibili.com/video/BV1Vr4y1G76c/)

---
layout: default
title: Runtime Library in Go
parent: Runtime Library
nav_order: 1
has_children: true
permalink: /runtime/go.html
---

# Runtime Library in Go

WATER Runtime Library for Go (a.k.a. `water-go`) is built with [wazero](https://github.com/tetratelabs/wazero) providing the WebAssembly runtime with WASI support, and implements an abstracted network programming interface that roughly ressembles the standard `net` package in Go. 

## Cross-platform Support

Project WATER is designed to be cross-platform and cross-architecture. See [Cross-platform Compatibility (Go)](./go/cross-platform.html) for more details.
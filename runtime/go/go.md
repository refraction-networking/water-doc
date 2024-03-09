---
layout: default
title: Runtime Library in Go
parent: Runtime Library
nav_order: 1
has_children: true
permalink: /runtime/go.html
---

# Runtime Library in Go: `water`
[![Go Doc](https://pkg.go.dev/badge/github.com/refraction-networking/water.svg)](https://pkg.go.dev/github.com/refraction-networking/water)

`water` (a.k.a. `water-go` in contrast to `water-rs`) is a WATER Runtime Library build in Go. It uses the WebAssembly runtime with WASI support from [wazero](https://github.com/tetratelabs/wazero), and implements an abstracted network programming interface that roughly ressembles the standard `net` package in Go. 

## Cross-platform Support
Project WATER is designed to be cross-platform and cross-architecture. See [Cross-platform Compatibility (Go)](./go/cross-platform.html) for more details.
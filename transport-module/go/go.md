---
layout: default
title: WATM in Go
parent: WebAssembly Transport Module
nav_order: 1
has_children: true
permalink: /transport-module/go.html
---

# WebAssembly Transport Module (WATM) in Go

Go is one of the most popular programming languages for building network applications, and it is possible to build (most of) Go applications into WebAssembly. 

Although the official Go compiler does not fully support WebAssembly System Interface (WASI) Preview 1's export feature, TinyGo, a Go compiler designed to build Go applications in a minimal environment, does provide great support for WASI.

## Quick Start

To see a quick example of how to build a WATM in Go, see [WATM in Go Quick Start](./go/quick-start.html).

## WATM Builder Helper

We provide a helper library you can use to build WATM from WebAssembly-agnostic Go code. By using this helper, only a simple intermediate layer is needed to be implemented in order to integrate existing transport written in Go with WATM.

See [WATM Builder Helper in Go](./go/watm.html) for more details, and see [WATM in Go Quick Start](./go/quick-start.html) for a quick example of using the helper to build a WATM that reverses strings.
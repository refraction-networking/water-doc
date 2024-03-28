---
layout: default
title: Runtime Library
nav_order: 3
has_children: true
permalink: /runtime.html
---

# Runtime Library

WATER provides runtime libraries in multiple programming languages to support the execution of [WebAssembly Transport Modules](/transport-module.html) (WATM) in different applications. A WATER runtime library includes a set of APIs providing a network programming interface for the integrating application to communicate with the WATM, and a WebAssembly runtime to compile/interpret and execute the WATM. 

## Available Runtime Libraries

Currently, we offer the following runtime libraries:
- Go: [water](https://github.com/refraction-networking/water)
- Rust: [water-rs](https://github.com/refraction-networking/water-rs)

Please see the documentation for each runtime library for more details.
---
layout: default
title: WebAssembly Transport Module
nav_order: 4
has_children: true
permalink: /transport-module.html
---

# WebAssembly Transport Module (WATM)

WebAssembly Transport Module (WATM) is a WebAssembly module that provides a set of APIs and act as a transport layer for applications integrating WATER. With the standardized WATM API, it is designed to be a lightweight and portable module that can be easily executed in any WATER Runtime.

## Build a WATM

Theoretically, you can write a WATM in any programming language that compiles to WebAssembly, since a WATM that fully implements the WATM API can be executed in any properly implemented WATER Runtime. 

For reference, we provide simple examples of WATMs written in Go and Rust, as well as helper libraries to simplify the development of WATMs in these languages. The ultimate goal is to provide a way to build a WebAssembly-agnostic transport protocol into a WATM without requiring the developer to understand the details of WebAssembly.
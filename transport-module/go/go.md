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
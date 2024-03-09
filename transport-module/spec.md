---
layout: default
title: WATM Specification
parent: WebAssembly Transport Module
nav_order: 3
---

# Current WebAssembly Transport Module (WATM) Specification
Currently we are in `v0` of the WebAssembly Transport Module (WATM) specification. This document describes the current version of the WATM specification.

## API 
The current version of WATM utilizes the WebAssembly System Interface (WASI) Preview 1 to provide the basic functionalities to interact between the WebAssembly module and the host environment. In addition, we define the following API for the transport module. 

### Imports

WATM imports the following functions from the host environment:

| Function Signature | Parameters | Results | Description |
| --- | --- | --- | --- |
|`host_dial` | - | `net_fd: s32` | Ask the host to connect to a remote server. |
| `host_accept` | - | `net_fd: s32` | Ask the host to accept an incoming connection. |
| `pull_config`* | - | `conf_fd: s32` | Ask the host to provide the configuration file. |
| ~~`host_defer`~~ | - | - | ~~Ask the host to defer the execution of the current function.~~ Deprecated: this function is equal to `NOP`. |

### Exports

WATM exports the following functions to the host environment:

| Function Signature | Parameters | Results | Description |
| --- | --- | --- | --- |
| `_water_v0` | - | - | Version 0 identifier. |
| `_water_init` | - | `errno: s32` | Initialize the transport module. |
| `_water_cancel_with` | `cancel_fd: s32` | `errno: s32` | Cancel the operation with the given file descriptor. |
| `_water_dial` | `internal_fd: s32` | `net_fd: s32` | Connect to a remote server. |
| `_water_accept` | `internal_fd: s32` | `net_fd: s32` | Accept an incoming connection. |
| `_water_associate` | - | `errno: s32` | Associate the transport module with the host environment. |
| `_water_worker` | - | `errno: s32` | Start the worker thread. |

## API Represented in Different Programming Languages
To help developers understand the API, we provide the following representations of the API in different programming languages that could be more intuitive than the above table.

### C representation
(planned feature)

### Go representation

<details>
    <summary>Imports</summary>
    
```go
//go:wasmimport env host_dial
//go:noescape
func _import_host_dial() (fd int32)

//go:wasmimport env host_accept
//go:noescape
func _import_host_accept() (fd int32)

//go:wasmimport env pull_config
//go:noescape
func _import_pull_config() (fd int32)

//go:wasmimport env host_defer
//go:noescape
func _import_host_defer()
```
</details>

<details>
    <summary>Exports</summary>

```go
//export _water_v0
func _water_v0()

//export _water_init
func _water_init()

//export _water_cancel_with
func _water_cancel_with(cancelFd int32) int32

//export _water_dial
func _water_dial(internalFd int32) (networkFd int32)

//export _water_accept
func _water_accept(internalFd int32) (networkFd int32)

//export _water_associate
func _water_associate() int32

//export _water_worker
func _water_worker() int32
```
</details>

### Rust representation
(to be added)

### WIT representation
Similarly, such API can be represented in WIT. 

_Note that the following is a simplified example and may not strictly follow the WIT syntax. In other words, we do not guarantee the following code to be compilable._

<details>
  <summary>Imports</summary>

```wit
// imports.wit
package env;

interface host {
    host_dial: func() -> (net_fd: s32);
    host_accept: func() -> (net_fd: s32);
    pull_config: func() -> (conf_fd: s32);
    host_defer: func(); // deprecated
}

world host-world {
    import host;
}
```
</details>

<details>
  <summary>Exports</summary>

```wit
// exports.wit
interface wasm {
    _water_v0: func(); // version 0 identifier
    _water_init: func() -> (errno: s32);
    _water_cancel_with(cancel_fd: s32) -> (errno: s32);
    _water_dial: func(internal_fd: s32) -> (net_fd: s32);
    _water_accept: func(internal_fd: s32) -> (net_fd: s32);
    _water_associate: func() -> (errno: s32); 
    _water_worker: func() -> (errno: s32);
}

world wasm-world {
    export wasm;
}
```
</details>
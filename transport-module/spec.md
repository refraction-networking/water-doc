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
| ~~`host_defer`~~ | - | - | ~~Ask the host to defer the execution of the current function.~~ Deprecated: this function is equal to `NOP`. |
| `pull_config`* | - | `conf_fd: s32` | Ask the host to provide the configuration file. |

### Exports

WATM exports the following functions to the host environment:

| Function Signature | Parameters | Results | Description |
| --- | --- | --- | --- |
| `_water_init` | - | `errno: s32` | Initialize the transport module. |
| `_water_dial` | `internal_fd: s32` | `net_fd: s32` | Connect to a remote server. |
| `_water_accept` | `internal_fd: s32` | `net_fd: s32` | Accept an incoming connection. |
| `_water_associate` | - | `errno: s32` | Associate the transport module with the host environment. |
| `_water_worker` | - | `errno: s32` | Start the worker thread. |

## API Represented in Different Programming Languages
To help developers understand the API, we provide the following representations of the API in different programming languages that could be more intuitive than the above table.

### C representation
(planned feature)

### Go representation
(to be added)

### Rust representation
(to be added)

### WIT representation
Similarly, such API can be represented in WIT. 

_Note that the following is a simplified example and may not strictly follow the WIT syntax. In other words, we do not guarantee the following code to be compilable._

#### Imported Functions
```wit
// imports.wit
package env;

interface host {
    host_dial: func() -> (net_fd: s32);
    host_accept: func() -> (net_fd: s32);
    host_defer: func(); // deprecated
    pull_config: func() -> (conf_fd: s32);
}

world host-world {
    import host;
}
```

#### Exported Functions
```wit
// exports.wit
interface wasm {
    _water_init: func() -> (errno: s32)
    _water_dial: func(internal_fd: s32) -> (net_fd: s32); 
    _water_accept: func(internal_fd: s32) -> (net_fd: s32); 
    _water_associate: func() -> (errno: s32); 
    _water_worker: func() -> (errno: s32); 
}

world wasm-world {
    export wasm;
}
```
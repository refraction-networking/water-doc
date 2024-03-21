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

`water` is a WATER Runtime Library build in Go. It uses the WebAssembly runtime with WASI support from [wazero](https://github.com/tetratelabs/wazero), and implements an abstracted network programming interface that roughly ressembles the standard `net` package in Go. 

## Cross-platform Support
Project WATER is designed to be cross-platform and cross-architecture. See [Cross-platform Compatibility (Go)](./go/cross-platform.html) for more details.

## Usage
This section introduces the basic usage of `water` in a Go project. For a fully working example, see [Quick Start with Runtime Library in Go](./go/quick-start.html).

### Importing `water`
To use `water` in a Go project, simply import it as a module.

By default, `water` does not recognize any transport modules as there can be many different 
specifications of transport modules. To use a specific transport module, import its implementation 
as well. 

```go
import (
	"github.com/refraction-networking/water"
	_ "github.com/refraction-networking/water/transport/v0" // import the v0 transport module spec
)
```

### Dialer Mode
Dialer acts like a client. It actively creates connections to a remote server (and usually is the one who sends the first message). 

```go
// Load the WebAssembly binary into wasm as []byte.
// The rest of the code on this page assumes that wasm is already loaded.
wasm, _ := os.ReadFile("./examples/v0/plain/plain.wasm")

config := &water.Config{
	TransportModuleBin: wasm,
}

dialer, _ := water.NewDialerWithContext(context.Background(), config)
conn, _ := dialer.DialContext(context.Background(),"tcp", remoteAddr)
```

### Listener Mode
Listener acts like a server. It listens on a network address and wait for 
incoming connections to accept. 

```go
lis, _ := config.ListenContext(context.Background(), "tcp", localAddr)
defer lis.Close()
log.Printf("Listening on %s", lis.Addr().String())

for {
	conn, err := lis.Accept()
	handleConn(conn)
}
```

### Relay Mode
A relay combines the functionalities of both a dialer and a listener. It works 
like a forward proxy, accepting connections from a client and forwarding them to a
remote server by dialing a connection to the remote server.

```go
relay, _ := water.NewRelayWithContext(context.Background(), config)
relay.ListenAndRelayTo("tcp", localAddr, "tcp", remoteAddr) // blocking
```
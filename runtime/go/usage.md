---
layout: default
title: Usage (Go)
grand_parent: Runtime Library
parent: Runtime Library in Go
nav_order: 3
---
# Usage

## Importing `water`
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

## Working Modes

### Dialer
Dialer acts like a client. It actively creates connections to a remote server (and usually is the one who sends the first message). 

```go
// ...

    // Load the WebAssembly binary into wasm as []byte.
    // The rest of the code on this page assumes that wasm is already loaded.
	wasm, _ := os.ReadFile("./examples/v0/plain/plain.wasm")

	config := &water.Config{
		TransportModuleBin: wasm,
	}

	dialer, _ := water.NewDialerWithContext(context.Background(), config)
	conn, _ := dialer.DialContext(context.Background(),"tcp", remoteAddr)

// ...
```

### Listener
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

### Relay
A relay combines the functionalities of both a dialer and a listener. It works 
like a forward proxy, accepting connections from a client and forwarding them to a
remote server by dialing a connection to the remote server.

```go
	relay, _ := water.NewRelayWithContext(context.Background(), config)

	relay.ListenAndRelayTo("tcp", localAddr, "tcp", remoteAddr) // blocking
```
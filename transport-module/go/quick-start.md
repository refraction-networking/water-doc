---
layout: default
title: Quick Start with WATM in Go
grand_parent: WebAssembly Transport Module
parent: WATM in Go
nav_order: 1
---

# Quick Start

This guide walks you through the process of building a simple WebAssembly transport module in Go with TinyGo, with the helper libraries provided by Project WATER.

## Prerequisites

- [TinyGo](https://tinygo.org/getting-started/) v0.31.0 or later
    - TinyGo v0.31.0 is used in this guide
- [Go](https://go.dev/) compatible with your TinyGo version
    - Go 1.22 is used in this guide
- Internet connection for Go to download the required packages

## Build a String Reversing WATM

First, create a new directory for your WATM project and navigate to it. Initialize your Go module with `go mod init` and required parameters. 

```go
package main

import (
	"io"

	v0 "github.com/refraction-networking/watm/tinygo/v0"
	v0net "github.com/refraction-networking/watm/tinygo/v0/net"
)

var dupBuf []byte = make([]byte, 4096) // 4KB buffer for reversing

// type guard: ReverseWrappingTransport must implement [v0.WrappingTransport].
var _ v0.WrappingTransport = (*ReverseWrappingTransport)(nil)

// Inside the init function, we register the ReverseWrappingTransport with 
// the helper libraries to enable all three types of transport modules: 
//  - Dialer
//  - Listener
//  - Relay
func init() {
	v0.BuildDialerWithWrappingTransport(&ReverseWrappingTransport{})
	v0.BuildListenerWithWrappingTransport(&ReverseWrappingTransport{})
	v0.BuildRelayWithWrappingTransport(&ReverseWrappingTransport{}, v0.RelayWrapRemote)
}

// main function is required for TinyGo to build the WATM. 
// It can be empty or contain any code you want to execute 
// when the WATM is loaded -- yes, this function will be 
// executed right after the WATM is instantiated.
func main() {}

type ReverseWrappingTransport struct {
}

func (rwt *ReverseWrappingTransport) Wrap(conn v0net.Conn) (v0net.Conn, error) {
	return &ReverseConn{conn}, conn.SetNonBlock(true) // must set non-block, otherwise will block on read and lose fairness
}

type ReverseConn struct {
	v0net.Conn // embedded Conn
}

func (rc *ReverseConn) Read(b []byte) (n int, err error) {
	n, err = rc.Conn.Read(dupBuf)
	if err != nil {
		return 0, err
	}

	if n > len(b) {
		err = io.ErrShortBuffer
		n = len(b)
	}

	// reverse all bytes read successfully so far
	for i := 0; i < n; i++ {
		b[i] = dupBuf[n-i-1]
	}

	return n, err
}

func (rc *ReverseConn) Write(b []byte) (n int, err error) {
	// reverse the bytes to be written
	for i := 0; i < len(b); i++ {
		dupBuf[i] = b[len(b)-i-1]
	}

	return rc.Conn.Write(dupBuf)
}
```

Save as `main.go` in your project directory. 

## Build

```bash
go mod tidy # to download the required packages
tinygo build -o reverse.wasm -target=wasi main.go
```

### Release Build with Optimization

With more precise control over the build process, 
you can use the following command to fine-tune 
the behavior in a release-level standard.

```bash
tinygo build -o reverse.wasm -target=wasi -no-debug -scheduler=none -gc=conservative main.go
```

Now you should end up with a `reverse.wasm` file in your project directory. It can be load into 
a [WATER runtime](/runtime.html) and used as a transport module.
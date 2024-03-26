---
layout: default
title: Quick Start with Runtime Library in Go
grand_parent: Runtime Library
parent: Runtime Library in Go
nav_order: 2
---

# Quick Start
This guide walks you through a few examples of using different modes of `water` Runtime Library.

## Dialer Mode
The code below demonstrates how to use `water` to create a dialer that connects to a remote server and send random bytes every 5 seconds.

```go
package main

import (
	"context"
	"crypto/rand"
	"flag"
	"fmt"
    "log"
	"net"
	"os"
	"time"

	"github.com/refraction-networking/water" // import the water package
	_ "github.com/refraction-networking/water/transport/v0" // explicitly enable WATM v0
)

var (
	remoteAddr = flag.String("raddr", "", "remote address to dial")
	wasmPath   = flag.String("wasm", "", "path to wasm file")
	remoteConn net.Conn
)

func main() {
	flag.Parse()

	wasm, err := os.ReadFile(*wasmPath)
	if err != nil {
		panic(fmt.Sprintf("failed to read wasm file: %v", err))
	}

	// start using W.A.T.E.R. API below this line, have fun!
	config := &water.Config{
		TransportModuleBin: wasm,
		NetworkDialerFunc:  net.Dial, // optional field, defaults to net.Dial
	}
	// configuring the standard out of the WebAssembly instance to inherit
	// from the parent process
	config.ModuleConfig().InheritStdout()
	config.ModuleConfig().InheritStderr()

	ctx := context.Background()
	// // optional: enable wazero logging
	// ctx = context.WithValue(ctx, experimental.FunctionListenerFactoryKey{},
	// 	logging.NewHostLoggingListenerFactory(os.Stderr, logging.LogScopeFilesystem|logging.LogScopePoll|logging.LogScopeSock))

	dialer, err := water.NewDialerWithContext(ctx, config)
	if err != nil {
		panic(fmt.Sprintf("failed to create dialer: %v", err))
	}

	conn, err := dialer.DialContext(ctx, "tcp", *remoteAddr)
	if err != nil {
		panic(fmt.Sprintf("failed to dial: %v", err))
	}
	defer conn.Close()
	// conn is a net.Conn that you are familiar with.
	// So effectively, W.A.T.E.R. API ends here and everything below
	// this line is just how you treat a net.Conn.

	remoteConn = conn

	worker()
}

func worker() {
	defer remoteConn.Close()

	log.Printf("Connected to %s", remoteConn.RemoteAddr())
	chanMsgRecv := make(chan []byte, 4) // up to 4 messages in the buffer
	// start a goroutine to read data from the connection
	go func() {
		defer close(chanMsgRecv)
		buf := make([]byte, 1024) // 1 KiB
		for {
			n, err := remoteConn.Read(buf)
			if err != nil {
				log.Printf("read remoteConn: error %v, tearing down connection...", err)
				remoteConn.Close()
				return
			}
			chanMsgRecv <- buf[:n]
		}
	}()

	// start a ticker for sending message every 5 seconds
	ticker := time.NewTicker(5 * time.Second)
	defer ticker.Stop()

	var sendBuf []byte = make([]byte, 4) // 4 bytes per message
	for {
		select {
		case msg := <-chanMsgRecv:
			if msg == nil {
				return // connection closed
			}
			log.Printf("peer: %x\n", msg)
		case <-ticker.C:
			n, err := rand.Read(sendBuf)
			if err != nil {
				log.Printf("rand.Read: error %v, tearing down connection...", err)
				return
			}
			// print the bytes sending as hex string
			log.Printf("sending: %x\n", sendBuf[:n])

			_, err = remoteConn.Write(sendBuf[:n])
			if err != nil {
				log.Printf("write: error %v, tearing down connection...", err)
				return
			}
		}
	}
}
```

## Listener Mode
The code below demonstrates how to use `water` to create a listener that listens on a local address and accepts incoming connections then sends random bytes every 5 seconds.

```go
package main

import (
	"context"
	"crypto/rand"
	"flag"
	"fmt"
    "log"
	"net"
	"os"
	"time"

	"github.com/refraction-networking/water"
	_ "github.com/refraction-networking/water/transport/v0"
)

var (
	localAddr = flag.String("laddr", "", "local address to listen on")
	wasmPath  = flag.String("wasm", "", "path to wasm file")
)

func main() {
	flag.Parse()

	wasm, err := os.ReadFile(*wasmPath)
	if err != nil {
		panic(fmt.Sprintf("failed to read wasm file: %v", err))
	}

	// start using W.A.T.E.R. API below this line, have fun!
	config := &water.Config{
		TransportModuleBin: wasm,
	}
	// configuring the standard out of the WebAssembly instance to inherit
	// from the parent process
	config.ModuleConfig().InheritStdout()
	config.ModuleConfig().InheritStderr()

	ctx := context.Background()
	// // optional: enable wazero logging
	// ctx = context.WithValue(ctx, experimental.FunctionListenerFactoryKey{},
	// 	logging.NewHostLoggingListenerFactory(os.Stderr, logging.LogScopeFilesystem|logging.LogScopePoll|logging.LogScopeSock))

	lis, err := config.ListenContext(ctx, "tcp", *localAddr)
	if err != nil {
		panic(fmt.Sprintf("failed to listen: %v", err))
	}
	defer lis.Close()
	log.Printf("Listening on %s:%s", lis.Addr().Network(), lis.Addr().String())
	// lis is a net.Listener that you are familiar with.
	// So effectively, W.A.T.E.R. API ends here and everything below
	// this line is just how you treat a net.Listener.

	clientCntr := 0
	for {
		conn, err := lis.Accept()
		if err != nil {
			panic(fmt.Sprintf("failed to accept: %v", err))
		}

		// start a goroutine to handle the connection
		go handleConn(fmt.Sprintf("client#%d", clientCntr), conn)
		clientCntr++
	}
}

func handleConn(peer string, conn net.Conn) {
	defer conn.Close()

	log.Printf("handling connection from/to %s(%s)", peer, conn.RemoteAddr())
	chanMsgRecv := make(chan []byte, 4) // up to 4 messages in the buffer
	// start a goroutine to read data from the connection
	go func() {
		defer close(chanMsgRecv)
		buf := make([]byte, 1024) // 1 KiB
		for {
			// conn.SetReadDeadline(time.Now().Add(5 * time.Second))
			n, err := conn.Read(buf)
			if err != nil {
				log.Printf("read %s: error %v, tearing down connection...", peer, err)
				conn.Close()
				return
			}
			chanMsgRecv <- buf[:n]
		}
	}()

	// start a ticker for sending message every 5 seconds
	ticker := time.NewTicker(5 * time.Second)
	defer ticker.Stop()

	var sendBuf []byte = make([]byte, 4) // 4 bytes per message
	for {
		select {
		case msg := <-chanMsgRecv:
			if msg == nil {
				log.Printf("read %s: connection closed, tearing down connection...", peer)
				return // connection closed
			}
			log.Printf("%s: %x\n", peer, msg)
		case <-ticker.C:
			n, err := rand.Read(sendBuf)
			if err != nil {
				log.Printf("rand.Read: error %v, tearing down connection...", err)
				return
			}
			// print the bytes sending as hex string
			log.Printf("sending: %x\n", sendBuf[:n])

			_, err = conn.Write(sendBuf[:n])
			if err != nil {
				log.Printf("write %s: error %v, tearing down connection...", peer, err)
				return
			}
		}
	}
}
```
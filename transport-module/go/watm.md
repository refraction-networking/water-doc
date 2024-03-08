---
layout: default
title: WATM Builder Helper in Go
grand_parent: WebAssembly Transport Module
parent: WATM in Go
nav_order: 2
---

# `watm`: WATM Builder Helper

[![Go Reference](https://pkg.go.dev/badge/github.com/refraction-networking/watm.svg)](https://pkg.go.dev/github.com/refraction-networking/watm)

We provide `watm` ([GitHub Repository](https://github.com/refraction-networking/watm)) as a helper library one can use to build WATM from WebAssembly-agnostic Go code. It would reduce a lot of boilerplate code and make the integration process much easier when you have an existing transport written in Go and does not want to customize the WATM's behavior too much. 

Currently, `watm` is still under active development and supports only TinyGo as the compiler. 

## `tinygo/v0` package

[![Go Reference](https://pkg.go.dev/badge/github.com/refraction-networking/watm.svg)](https://pkg.go.dev/github.com/refraction-networking/watm/tinygo/v0)

The `tinygo/v0` package provides a set of helper functions to build WATM with spec version 0 from WebAssembly-agnostic Go code. It is designed to be used with TinyGo.

<!-- TODO: Add documentations to API Reference? -->
<details>
  <summary>API Reference</summary>

```go
type RelayWrapSelection bool

const (
    RelayWrapRemote RelayWrapSelection = false
    RelayWrapSource RelayWrapSelection = true
)

type ConfigurableTransport interface {
    Configure([]byte) error
}

type DialingTransport interface {
    SetDialer(dialer func(network, address string) (v0net.Conn, error))
    Dial(network, address string) (v0net.Conn, error)
}


type ListeningTransport interface {
    SetListener(listener v0net.Listener)
    Accept() (v0net.Conn, error)
}

type WrappingTransport interface {
    Wrap(v0net.Conn) (v0net.Conn, error)
}

func BuildDialerWithDialingTransport(DialingTransport)
func BuildDialerWithWrappingTransport(WrappingTransport)

func BuildListenerWithListeningTransport(ListeningTransport)
func BuildListenerWithWrappingTransport(WrappingTransport)

func BuildRelayWithListeningDialingTransport(ListeningTransport, DialingTransport)
func BuildRelayWithWrappingTransport(WrappingTransport, RelayWrapSelection)
```

</details>

### `tinygo/v0/net` package 

[![Go Reference](https://pkg.go.dev/badge/github.com/refraction-networking/watm.svg)](https://pkg.go.dev/github.com/refraction-networking/watm/tinygo/v0/net)

The `tinygo/v0/net` package simulates the standard `net` package in Go, and provides a set of helper functions to build WATM with spec version 0 from WebAssembly-agnostic Go code. It is designed to be used with TinyGo. 

If you are using `tinygo/v0` properly, then most likely you don't have to interact with this package too much. However, more flexibility is provided here if you want to customize the behavior of the WATM.


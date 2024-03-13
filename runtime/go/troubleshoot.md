---
layout: default
title: Troubleshooting (Go)
grand_parent: Runtime Library
parent: Runtime Library in Go
nav_order: 2
---
# Troubleshooting

## Enable `wazero` debug logs

`wazero` is the WebAssembly runtime with WASI support that `water` uses. To enable debug logs from `wazero`, pass the values below via the `context.Context` 

```go
// example of enabling FileSystem, Poll, and Sock logging scopes of wazero
ctx = context.WithValue(ctx, experimental.FunctionListenerFactoryKey{},
	logging.NewHostLoggingListenerFactory(os.Stderr, logging.LogScopeFilesystem|logging.LogScopePoll|logging.LogScopeSock))
```
---
layout: default
title: Cross-platform Compatibility (Go)
grand_parent: Runtime Library
parent: Runtime Library in Go
nav_order: 1
---

# Cross-platform Support

The end goal of Project WATER is to provide a universal pluggable network transport layer that harnesses the power of WebAssembly to enable the Rapid Deployment of new network transport protocols on even the least flexible platforms such as smartphones with strict and slow app store review processes.

To achieve this, Project WATER is designed to be cross-platform and cross-architecture. The WATER maintainers are dedicated to expanding the cross-platform support of the runtime libraries to as many platforms as possible.

## Platform Support Matrix

Currently, `water` supports the following platforms: 

|       Target       | Compiles? | Tests Pass? |
| ------------------ | --------- | ----------- | 
| linux/amd64        | ✅        | ✅         |
| linux/arm64        | ✅        | ✅         |
| linux/riscv64      | ✅        | ✅         |
| macos/amd64        | ✅        | ✅         |
| macos/arm64        | ✅        | ✅         |
| windows/amd64      | ✅        | ✅         |
| windows/arm64      | ✅        | ❓         |
| others             | ❓        | ❓         |

Due to the absence of a machine in `windows/arm64`, we could not run tests on this platform. The compilation compatibility is tested by cross-compiling from `windows/amd64` to `windows/arm64`. 
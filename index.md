---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

layout: home
title: WATER
nav_order: 0
---

-- **WATER** -- <br> **W**eb**A**ssembly **T**ransport **E**xecutables **R**untime
{: .fs-8}
{: .fw-300}
{: .text-grey-dk-300}
{: .text-center}

WATER is all about a new way to **build**, **deploy**, and **execute** new and updated pluggable transports. With WebAssembly, transport protocols could be written in any language that compiles to WebAssembly, distributed via multiple channels, and run in a secure, sandboxed environment in a tool written in a different programming language on a wide range of platforms.

## Pluggable Transports and Censorship Circumvention

Pluggable Transports provides a flexible and modular way for censorship circumvention tools to integrate new transport protocols and/or update existing ones. Multiple popular circumvention tools, such as Tor, Psiphon, and Lantern, have adopted the pluggable transport design. However, the flexibility of pluggable transports is still limited. For example, the host application and the transport protocol must be written in the same programming language, and the transport protocol must be compiled into the host application, which makes deploying new transport protocols and updating existing ones not so efficient. 

### A more pluggable transport

WATER provides much more flexibility compared to existing pluggable transport designs. A matrix comparing WATER with other pluggable transport design approaches is available [here](comparisons.html#water-vs-other-pluggable-transport-designs).

## How does WATER work?

The ultimate goal of WATER is to provide familiar network programming interfaces in multiple different programming languages which are backed by the WebAssembly-based pluggable transport modules. 

<p align="center">
  <img src="/assets/img/flows.png" alt="WATER Workflow" width="512"/>
</p>

With a WebAssembly Transport Module (WATM), WATER can be configured for either client-side or server-side applications as it provides both dialer and listener implementations. It is worth noting that a WATM may be written in a way to support either client-side or server-side applications, or both. Please consult the provider of the WATM for more information.

The architecture of WATER, especially how WATER is built, is available [here](architecture.html).
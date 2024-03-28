---
layout: page
title: Architecture
permalink: /architecture.html
nav_order: 2
---

# Architecture

<p align="center">
  <img src="/assets/img/ring.png" alt="WATER Ring" width="512"/>
</p>

As mentioned, WATER provides a familiar network programming interface for every supported programming language, and use WebAssembly as the underlying technology to build and execute transport protocols. The architecture of WATER could be illustrated as a ring. The inner-most ring (violet) stands for the WebAssembly* Transport Module (WATM), a WebAssembly binary module which defines the transport protocol. The middle ring (blue) stands for the WATER Runtime, which consists of a set of internal APIs interfacing the WATM, and external APIs providing the said network programming interface. The outer-most ring (grey) stands for the integrating application, which could be written in any programming language with a WATER Runtime library available.

*That's why it is violet. 
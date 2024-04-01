---
layout: page
title: Comparisons
permalink: /comparisons.html
nav_order: 1
---

# Comparisons

## WATER vs. other pluggable transport designs

We evaluate WATER against other pluggable transport designs in terms of the following features:
- **Hot-swappable**: Can the transport be loaded and unloaded without recompling the host application?
- **Host Isolation**: Does the transport execute in an isolated environment (e.g., a sandbox) and with limited access to the host resources? 
- **Compatibility Guarantee**: Can the host application be transport-agnostic and still work with any transport that implements the interface?
- **Cross-language Deployment**: Can the transport be implemented in a different programming language than the host application?


|    Feature     | PTs | Marionette | Proteus | WATER |
|----------------|------|------------|---------|-------|
| Hot-swappable  | ❌ | ✅ | ✅ | ✅ |
| Host Isolation | ❌ | ⚠️ | ✅ | ✅ |
| Compatibility Guarantee | ❌ | ⚠️ | ✅ | ✅ |
| Cross-language Deployment | ❌ | ❌ | ❌ | ✅ |

### References
- PTs ([pluggabletransports.info](https://www.pluggabletransports.info/))
- Marionette: A Programmable Network Traffic Obfuscation System ([USENIX Security'15](https://www.usenix.org/conference/usenixsecurity15/technical-sessions/presentation/dyer))
- Proteus: Programmable Protocols for Censorship Circumvention ([FOCI 2023](https://www.petsymposium.org/foci/2023/foci-2023-0013.php))

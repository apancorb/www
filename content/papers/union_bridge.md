---
title: "Union: A Trust-minimized Bridge for Rootstock"
author: ["Ramon Amela", "Shreemoy Mishra", "Sergio Demian Lerner", "Javier Alvarez Cid-Fuentes"]
ShowReadingTime: false
cover:
    image: /papers/union_bridge.png
    caption: "[Union: A Trust-minimized Bridge for Rootstock](https://arxiv.org/pdf/2501.07435)"
tags: ["bitcoin", "distributed systems", "blockchain systems", "bridge", "rootstock"]
draft: false
---

We present Union, a trust-minimized bridge protocol that enables secure transfer
of BTC between Bitcoin and Rootstock. The growing ecosystem of
blockchain systems built around Bitcoin has created a pressing need for secure
and efficient bridges to transfer BTC between networks while preserving Bitcoin’s
security guarantees. Union employs a multi-party variant of BitVMX, an optimistic
proving system on Bitcoin, to create a bridge that operates securely under
the assumption that at least one participant remains honest. This `1-of-n` honest
approach is strikingly different from the conventional honest-majority assumption
adopted by practically all federated systems. The protocol introduces several
innovations: a packet-based architecture that allows security bonds to be reused
for multiple bridge operations, improving capital efficiency; a system of enablers
to manage functionaries participation and to enforce penalties; a flexible light
client framework adaptable to various blockchain architectures; and an efficient
stop watch mechanism to optimize time-lock management. Union is a practical
and scalable solution for Bitcoin interoperability that maintains strong security
guarantees and minimizes trust assumptions.

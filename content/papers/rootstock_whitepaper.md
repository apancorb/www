---
title: "Rootstock: Bitcoin Powered Smart Contracts"
author: "Sergio Demian Lerner"
ShowReadingTime: false
cover:
    image: /papers/rootstock_whitepaper.png
    caption: "[Rootstock: Bitcoin Powered Smart Contracts](https://rootstock.io/rsk-white-paper-updated.pdf)"
tags: ["bitcoin", "rootstock", "blockchain systems", "crypto", "smart contracts"]
draft: false
---

RSK (Rootstock) is a Bitcoin sidechain launched in 2018 that enables smart contract 
functionality while using Bitcoin as its native asset, addressing Bitcoin's limited
programmability without requiring a new speculative token. Built as an evolution of
QixCoin and Ethereum concepts, RSK provides a Turing-complete virtual machine (RVM)
that's highly compatible with Ethereum's tools and dApps, offering faster transaction
confirmations (around 30 seconds) while maintaining Bitcoin's security through SHA-256D
merged mining with over 40% of Bitcoin's hash rate as of 2019. The platform uses a two-way
peg system where Bitcoins become "Smart Bitcoins" (RBTC) on the RSK blockchain and can be
converted back without additional costs beyond standard fees, with no new currency issuance
since all RBTC originates from actual Bitcoin. RSK currently operates with a federated
peg and includes protection against selfish mining through the DECOR+ protocol, while
the community has planned future upgrades including storage rent, parallel transaction
processing, block propagation optimizations, transaction compression protocols, support
for additional VMs, and a hybrid federation/drivechain-based peg system, all documented
as RSK Improvement Proposals (RSKIPs) in their GitHub repository, with RSK Labs serving
as the primary development company since 2015.

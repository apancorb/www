---
title: "Enabling Blockchain Innovations with Pegged Sidechains"
author: ["Adam Back", "Matt Corallo", "Luke Dashjr", "Mark Friedenbach", "Gregory Maxwell", "Andrew Miller", "Andrew Poelstra", "Jorge Timón", "Pieter Wuille"]
ShowReadingTime: false
cover:
    image: /papers/enabling_blockchain_innovations_with_pegged_sidechains.png
    caption: "[Enabling Blockchain Innovations with Pegged Sidechains](https://blockstream.com/sidechains.pdf)"
tags: ["blockchain", "sidechain", "bitcoin", "simplified payment verification", "protocol"]
draft: false
---

Since the introduction of Bitcoin in 2009, and the multiple computer science
and electronic cash innovations it brought, there has been great interest in the 
potential of decentralised cryptocurrencies. At the same time, implementation changes
to the consensuscritical parts of Bitcoin must necessarily be handled very conservatively.
As a result, Bitcoin has greater difficulty than other Internet protocols in adapting
to new demands and accommodating new innovation.

We propose a new technology, pegged sidechains, which enables bitcoins and other ledger
assets to be transferred between multiple blockchains. This gives users access to new and
innovative cryptocurrency systems using the assets they already own. By reusing Bitcoin’s
currency, these systems can more easily interoperate with each other and with Bitcoin, avoiding
the liquidity shortages and market fluctuations associated with new currencies. Since sidechains
are separate systems, technical and economic innovation is not hindered. Despite bidirectional
transferability between Bitcoin and pegged sidechains, they are isolated: in the case of a
cryptographic break (or malicious design) in a sidechain, the damage is entirely confined to
the sidechain itself.

This paper lays out pegged sidechains, their implementation requirements, and the work
needed to fully benefit from the future of interconnected blockchains.

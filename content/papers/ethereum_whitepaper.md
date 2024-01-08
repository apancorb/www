---
title: "Ethereum: A Next Generation Smart Contract & Decentralized Application Platform"
author: "Vitalik Buterin"
ShowReadingTime: false
cover:
    image: papers/ethereum_whitepaper.png
    caption: "[Ethereum: A Next Generation Smart Contract & Decentralized Application Platform](https://ethereum.org/en/whitepaper/)"
tags: ["ethereum", "distributed systems", "blockchain systems", "smart contracts", "computer science"]
draft: false
---

When Satoshi Nakamoto first set the Bitcoin blockchain into motion in January 2009,
he was simultaneously introducing two radical and untested concepts. The first is
the "bitcoin", a decentralized peer-to-peer online currency that maintains a value
without any backing, intrinsic value or central issuer. So far, the "bitcoin" as 
a currency unit has taken up the bulk of the pu blic attention, both in terms of
the political aspects of a currency without a central bank and its extreme upward
and downward volatility in price. However, there is also another, equally important,
part to Satoshi's g rand experiment: the concept of a proof of work based blockchain
to allow for public agreement on the order of transactions. Bitcoin as an application
can be described as a first-to-file system: if one entity has 50 BTC, and simultaneously
sends the same 50 BTC to A and to B, only the transaction that gets confirmed first
will process. There is no intrinsic way of determining from two transactions which
came earlier, and for decades this stymied the development of decentralized digital
currency. Satoshi's blockchain was the first credible decentralized solution. And now,
attention is rapidly starting to shift toward this second part of Bitcoin's technology,
and how the blockchain concept can be used for more than just money.

Commonly cited applications include using on-blockchain digital assets to represent
custom currencies and financial instruments ("colored coins"), the ownership of an
underly ing physical device ("smart property"), non-fungible assets such as domain
names ("Namecoin") as well as more adva nced applications such as decentralized exchange,
financial derivatives, peer-to-peer gamblin g and on-blockchain identity and reputation
systems. Another important area of inquiry is "smart contract s" - systems which
automatically move digital assets according to arbitrary pre-specified rules. For
example, one might have a treasury contract of the form "A can withdraw up to X
currency units per day, B can withdraw up to Y per day, A and B together can withdraw
anything, and A can shut off B's ability to withdraw". Th e logical extension of this is
decentralized autonomous organizations (DAOs) - long-term smart con tracts that
contain the assets and encode the bylaws of an entire organization. What Ethereum
intends to provide is a blockchain with a built-in fully fledged Turing-complete
programming language that can be used to c reate "contracts" that can be used
to encode arbitrary state transition functions, allowing users to create any of
the systems described above, as well as many others that we have not yet imagined,
simply by writing up the logic in a few lines of code.

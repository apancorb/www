---
title: "I Found 0.34 BTC on a Friend’s USB Drive"
author: "Antonio Pancorbo"
summary: "A Lost Wallet.dat, Forgotten Since 2017"
date: 2024-11-30
cover:
    image: "/posts/lost_bitcoin_usb.png"
tags: ["bitcoin", "wallet", "blockchain", "story"]
featured: true
featuredWeight: 1
draft: false
---

## ☀️  The Summer Conversation That Sparked It All

It was a chill summer afternoon in 2024. I had recently met up with an old friend,
and as we chatted about work and life, I mentioned that I was a software engineer
working mostly in blockchain.

That’s when **my friend and her mother** lit up.

They told me that her brother had been involved with Bitcoin "a long time ago."
He used to mine, but they didn’t really know the details. The laptop that held
their funds had died years earlier. They took it to a local IT shop, where the
technician managed to extract the hard drive. But the brother suspected something
shady — he believed the IT guy had stolen the Bitcoin. Since then, they assumed
the bitcoin were gone for good and didn’t speak about it again.

But something in me clicked. I insisted — for a couple of months — that they try
to recover any files they might still have. “Just let me check,” I told them.
“There’s always a chance.”

---

## 💾 A USB With More Than Music

In **late November 2024**, my friend finally handed me a dusty USB drive from 
her brother.

To be honest, I wasn’t expecting anything. There were random music files on it
— like 2000s playlists — so I assumed this was going to be a dead end. But then
I saw it: `wallet.dat`.

And not just one — there were several. Most were empty. But one… one had everything.

Even better, it wasn’t encrypted. That was pure luck. The brother clearly had no
idea how Bitcoin wallets worked technically, and if it had required a passphrase,
the story would’ve ended right there. So very happy :)

---

## ⚙️  Digging In With Bitcoin Core

I loaded the wallet into Bitcoin Core by starting `bitcoind` and using this command:

```bash
$ bitcoin-cli listaddressgroupings
```

But at first glance, this was the output:

```json
[
  [
    [
      "125yAhYvKi338k8ckHq2MAu6AuHsxt2AKb",
      0.0
    ]
  ],
  [
    [
      "147wz3yigMEgXMHDngLunf9E36PuvFVZ5J",
      0.0
    ]
  ],
  [
    [
      "16LD9SHAmpxFYoEgHsrV5wYRcSCQYdtXTH",
      0.0
    ]
  ],
  [
    [
      "18LyYWexgkEfp4pXzfdjkr8e8puPXotcQi",
      0.0
    ]
  ],
  [
    [
      "15UxHKuVDvb4is8WLqvZwwmmDHuLrHA5HJ",
      0.0
    ]
  ],
  [
    [
      "1CqFBxkL2K6ofkcq4aRCsuoUaBEwcYiN7s",
      0.0
    ]
  ],
  [
    [
      "1CcuztN4f4Cfd87mbGjTXCoAEynMzu3SZi",
      0.0
    ]
  ]
]
```

Seeing all those zero balances was disappointing — at first. But then I remembered:
**my node wasn’t synced**, so it couldn’t possibly know what was on-chain.

So I pasted one of the addresses into [mempool.space](https://mempool.space/address/125yAhYvKi338k8ckHq2MAu6AuHsxt2AKb),
and boom — it had transactions. The detective work had just begun.

---

## 🔐 Dumping Private Keys & Loading into Electrum

Once I realized the wallet was legitimate, I exported all private keys with:

```bash
$ bitcoin-cli dumpwallet /tmp/wallet-keys.txt
```

I loaded them into [Electrum Wallet](https://electrum.org), and this was the result:

[!["Electrum Screenshot"](/posts/lost_bitcoin_electrum_screenshot.png#center)](https://www.tony.software/posts/lost_bitcoin_electrum_screenshot.png.png)

Each line told a story — from early 2017 interactions to 2018 transfers. Slowly,
balances started stacking up.

I mapped out every address, every transaction, and kept personal notes.
It was like digital archaeology.

Some funds came from Poloniex.com, others from a multisig wallet — classic signs
of a mining pool payout structure. The timestamps, transaction trails, and
patterns all pointed to that.

Even though they didn’t mine blocks directly, they contributed to a pool and
got their fair share.

---

## 💥 The Big One: $16,000 Worth of BTC

I still remember the address:

**1CcuztN4f4Cfd87mbGjTXCoAEynMzu3SZi**

It received **0.14883330 BTC** in June 2018 — worth around **$16,000 USD** at
today’s prices.

That moment was pure disbelief. Until that point, I had seen addresses with maybe
$100 each. Then suddenly… bam. I knew her brother was going to be very happy!

---

## 🍷 A Cold Wallet and a Glass of Wine

I met up with the brother at a bar — we both ordered a glass of wine. He had already
bought a **Trezor hardware wallet**, so I helped him move the funds into cold
storage using Electrum.

Here’s the final transaction:

**Transaction:** [View on Mempool](https://mempool.space/tx/8bd10c641f50150e00b518ac24914f42ffbbe14bf1c7d804ef0af145a9e5d7f6)  
**Recipient address:** [`bc1q7xaen8u2742kzj2459cfrgyqyyme4a2adhpmmj`](https://mempool.space/address/bc1q7xaen8u2742kzj2459cfrgyqyyme4a2adhpmmj)  
**Amount:** `0.34862022 BTC`  
> _(Go ahead, click and check if it’s still there — let’s hope he remembered the seed phrase!)_

---

## 🧾 Final Thoughts

This was one of the most rewarding and fun technical adventures I’ve had in a while.

The biggest takeaway?  

**Backup your wallets** — and don’t throw away strange USB drives just because they
look empty. That binary blob might hold life-changing value.

Her brother didn’t even know what `wallet.dat` was… yet it held over **0.34 BTC**
since 2017.

And the best part?  

It wasn’t encrypted 😅

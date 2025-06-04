---
title: "The CAP Theorem and Blockchain Systems"
author: "Antonio Pancorbo"
summary: "CAP Theorem and its use with different types of distributed systems such as blockchain systems"
date: 2023-12-09
cover:
    image: /posts/cap_theorem.png
tags: ["cap", "distributed systems", "blockchain systems", "blockchain trilemma", "system design"]
draft: false
---

During the 2000 Symposium on Principles of Distributed Computing, Eric Brewer
delivered a keynote speech detailing the transformative shifts in distributed
database development. Prior to his talk, the exponential growth in data volume
necessitated more scalable solutions beyond traditional ACID-based databases.
Consequently, novel principles emerged, encapsulated within the BASE paradigm,
emphasizing basic availability, soft-state, and eventual consistency. Brewer
scrutinized the repercussions of this paradigm shift, leading to the formulation
of the CAP Theorem, initially posited as a personal hypothesis rather than an
empirically proven statement. Despite its speculative nature, the theorem sparked
widespread interest among researchers, culminating in its formal proof within two
years.

Over time, the CAP Theorem has undergone continuous refinement, with Brewer himself
revisiting the subject in a subsequent paper. He amended certain conclusions,
acknowledging that while not erroneous, they might lead to potential misinterpretations.
Notably, Brewer's 2012 paper, "CAP twelve years later: How the 'rules' have changed,"
highlighted these nuances. Nonetheless, the CAP Theorem remains one of the foundational
discoveries in the realm of distributed databases, significantly influencing their
design and operation.

The CAP theorem states that any networked shared-data system can have at most two
of three desirable properties:

* consistency (C) equivalent to having a single up-to-date copy of the data
* high availability (A) of that data (for updates)
* tolerance to network partitions (P)

Opting to abandon Partition Tolerance lacks practicality within real-world scenarios
due to the inevitability of network partitions. Consequently, the choice typically
narrows down to prioritizing between Availability and Consistency, embodied respectively
by ACID (Consistency) and BASE (Availability) principles. Yet, Brewer's insight
acknowledged that this decision isn't a stark binary one. The spectrum between
these extremes holds considerable value; blending varying degrees of Availability
and Consistency often leads to more optimal outcomes.

In blockchain systems, such as Bitcoin, network partitions occur frequently since
the nodes that form the system are globally distributed across many unreliable
networks. Therefore, out of Consistency and Availability, blockchain systems
select Availability reaching Consistency eventually. Why? If a blockchain system
sacrifices availability, this means that the system pauses when transactions can’t
be committed. So, if there’s a partition, a user can’t read or write data at all.
In other words, if a blockchain system chose Consistency over Availability, it
would mean that it’d become unavailable to users whenever it had to resolve an
issue with consistency.

Hence, it can be asserted that blockchain systems align with AP (Available and
Partition-tolerant) databases, resembling Dynamo. Dynamo, an entirely decentralized
Key-value Store created by Amazon, prioritizes achieving robust availability. It
integrates the fundamental "eventual consistency" principle from BASE, employing
a decentralized replica synchronization protocol that upholds consistency during
updates via a quorum-like method and object versioning. The system effectively
detects failures through gossip protocols.

Blockchain systems attain consistency as transactions within a block receive
additional confirmations. Put simply, the greater the number of subsequent blocks
built atop a specific block, the stronger the assurance becomes that all nodes
within the system hold an identical data state or share a consistent view of the
distributed ledger. Consequently, the deeper a transaction is embedded in the chain,
the lower the probability of any alterations occurring.

More recently, Vitalik Buterin applied CAP theorem to the blockchain, and coined
the phrase *“Blockchain Trilemma”*. This describes a similar dilemma faced by blockchain
system designers when juggling three conflicting requirements: decentralization,
security, and scalability.

Blockchain networks inherently operate in a decentralized manner, devoid of singular
control by any individual or entity. The level of decentralization in these networks
varies across a spectrum. Security in a blockchain pertains to its resistance against
malicious tampering attempts by bad actors. On the other hand, scalability denotes a
blockchain's capacity to handle elevated transactional volumes without experiencing
a decline in performance as its adoption and usage intensify.

Thus, it may be the case that the Blockchain Trilemma is a better method to understand
and design blockchain systems than the CAP theorem. 

---

### References
1. Hellwig, D., Karlic, G. and Huchzermeier, A. (2020) Build your own blockchain.
Springer International Publishing. 
2. Simon, S. (no date) ‘Brewer’s CAP Theorem’. 
3. Brewer, E. (2012, February). CAP twelve years later: How the "rules" have
changed. Computer, vol. 45, no. 2 , pp. 23-29.
4. Brewer, E. (2000). Towards Robust Distributed System. Symposium on Principles
of Distributed Computing (PODC).
5. DeCandia, G., & al. (2007). Dynamo: Amazon's Higly Available Key-value Store.
ACM Press New York, pp. 205-220.
6. Gilbert, S., & Lynch, N. (2002, June). Brewers Conjunction and the Feasability
of Consistent, Available, Partition-Tolerant Web Services. ACM SIGACT News , p. 33(2).
7. Sregantan, N. (no date) What is the blockchain trilemma?: DBS Singapore, DBS.
Available [here](https://www.dbs.com.sg/personal/articles/nav/investing/what-is-the-blockchain-trilemma#:~:text=The%20Blockchain%20Trilemma%20refers%20to,Security%2C%20scalability%2C%20and%20decentralisation).


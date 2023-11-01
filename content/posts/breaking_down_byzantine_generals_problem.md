---
title: "Breaking Down the Byzantine Generals Problem"
author: "Antonio Pancorbo"
summary: "Exploration of the Byzantine Generals Problem and its role in building reliable distributed systems"
date: 2023-09-15
cover:
    image: /posts/byzantine_general_belisarius.jpg
    caption: "Byzantine General Belisarius, Having is a drawing by Mary Evans Picture Library"
tags: ["distributed systems", "byzantine generals problem", "computer networks", "fault tolerance", "reliability"]
draft: false
---

Most distributed systems that utilize a client-to-server network architecture, such
as those found in distributed databases systems, make the assumption that each
node in the system will always be honest. They may be slow or never respond
due to many different reasons, such as a fault or network delays, but to their
best of their knowledge they will always follow the protocol in order to make
the system work correctly. Therefore, reaching consensus in such an environment
is "easier" considering that most existing systems use consensus algorithms
that assume full honestly of all the nodes. The problem of reaching consensus
becomes harder when dealing with a peer-to-peer network architecture where each node
is distributed across the world and controlled by many independent parties. In this case,
honesty of the nodes cannot be assumed. When a node in a distributed system willingly
sends arbitrary faulty or corrupted messages, is known as a Byzatine fault. The
problem of reaching consensus in such an envirioment is known as the 
*Byzatine Generals Problem*.

In the Byzantine Generals Problem, multiple divisions of the Byzantine army are
encamped outside an adversary's city, each led by its own general. These commanders
have a limited means of communication via messengers. Their challenge is to strategize
collaboratively after assessing the enemy's situation. The complicating factor is that
a portion of these generals might be traitors with intentions to disrupt any unified
agreement among the loyal ones. I personally find this problem to be very interesting,
and what follows next is an overview of the problem presented in the formal [paper](https://lamport.azurewebsites.net/pubs/byz.pdf).

The Byzantine General Problem is formalized as follows: A commanding general must
send an order to his `n - 1` lieutenant generals such that:
1. All loyal lieutenants obey the same order.
2. If the commanding general is loyal, then every loyal lieutenant obeys the
order he sends.

The Byzantine Generals Problem may appear deceptively straightforward, but its
complexity becomes evident when you realize that, if the generals can only communicate
through oral messages, a solution requires the loyalty of more than two-thirds of
the generals. Notably, even with just three generals, a solution is impossible
when there's a single traitor among them. An oral message, in this context, is
entirely controlled by the sender, allowing a traitor to transmit any message,
resembling typical computer communications.

In order to understand why it is needed the loyalty of more than two-thirds of
the generals, consider the following two simple examples.

[!["Byzantine Generals Problem Example"](/posts/byzantine_generals_problem.png#center)](https://www.tony.software/posts/byzantine_generals_problem.png)

To simplify, we'll consider a scenario where the only available choices are "attack"
or "retreat." Let's start with Figure 1, where the commander is loyal and orders
an "attack," but Lieutenant 2 is a traitor and falsely reports receiving a "retreat"
command to Lieutenant 1. To satisfy the second condition, Lieutenant 1 must follow
the "attack" order, but he cannot be certain that order is the correct one.

Now, take a look at Figure 2, where the commander is the traitor. He sends an "attack"
command to Lieutenant 1 and a "retreat" order to Lieutenant 2. Lieutenant 1 is
unaware of the traitor's identity and can't verify what message the commander sent
to Lieutenant 2. Consequently, both scenarios in these pictures appear identical to
Lieutenant 1. If the traitor consistently lies, Lieutenant 1 can't distinguish
between these situations, obliging him to follow the "attack" order in both cases
when he receives such a command from the commander.

However, a similar reasoning demonstrates that if Lieutenant 2 receives a "retreat"
command from the commander, he must follow it, even if Lieutenant 1 claims the
commander said "attack." As a result, in the Figure 2 scenario, Lieutenant 2 obeys
the "retreat" order while Lieutenant 1 follows the "attack" order, violating the
first condition. Consequently, no solution is feasible for three generals when
faced with a single traitor.

---

The good news is that the Byzantine Generals Problem can be solved. In one
solution, we examine scenarios where message forgery is a possibility. This
approach maintains Byzantine fault tolerance as long as the number of disloyal
generals remains below one third of the total number of generals. In other words,
there must be at least `3m + 1` loyal generals where `m` is the number of traitors.

Consider the following case:

[!["Byzantine Generals Problem Oral Solution"](/posts/byzantine_generals_problem_oral.png#center)](https://www.tony.software/posts/byzantine_generals_problem_oral.png)

To comprehend the inner workings of this solution, let's focus on a specific scenario
with `m = 1` and `n = 4`. In Figure 3, we can see the messages received by Lieutenant 2 when the
commander transmits the value `v` and that Lieutenant 3 is acting as a traitor.
The commander dispatches `v` to all three lieutenants. Later on, Lieutenant 1 sends
the value `v` to Lieutenant 2. Simultaneously, the treacherous Lieutenant 3 sends
a different value, denoted as `x`, to Lieutenant 2. Lieutenant 2 now possesses
`v1 = v2 = v` and `v3 = x`. In this situation, he can accurately determine the 
correct value `v` by taking the majority among `v`, `v`, and `x` using the formula
`v = majority(v, v, x)`.

Moving on, let's explore the scenario where the commander acts as a traitor. Figure 4
illustrates the values received by the lieutenants when a treacherous commander
sends three arbitrary values: `x`, `y`, and `z`, to the three lieutenants. In this
situation, each lieutenant ends up with the following values: `v1 = x`, `v2 = y`,
and `v3 = z`. Since there is not a majority between the values, the generals will
simply default or retreat.

As we've observed from the scenarios presented in Figures 1 and 2, the Byzantine
Generals Problem's complexity primarily arises from the traitors' capability to
deceive. This problem becomes more tractable when we can limit their capacity to
lie. One effective approach is to enable the generals to transmit messages that
cannot be forged. With the introduction of these unforgeable signed messages,
our earlier argument necessitating four generals to handle one traitor no longer
applies. In fact, a solution involving just three generals is now feasible.
In the following, we'll outline a solution designed to handle `m` traitors
for any number of generals.

Consider the following case:

[!["Byzantine Generals Problem Signed Solution"](/posts/byzantine_generals_problem_signed.png#center)](https://www.tony.software/posts/byzantine_generals_problem_signed.png)

In the solution, the commander initiates the process by sending a signed order
to each of his lieutenants. Subsequently, each lieutenant appends their own
signature to this order and forwards it to the other lieutenants. This process 
continues iteratively, with each lieutenant receiving a signed message, creating
multiple copies, signing them, and dispatching them further. We let `x : i` denote
the value `x` signed by General `i`. Thus, `v : j : i` denotes the value `v` signed
by `j`, and then that value `v : j` signed by `i`.

Figure 5 illustrates the case of three generals when the commander acts as a traitor.
In this scenario, the commander transmits an "attack" order to one lieutenant and a
"retreat" order to the other. Both lieutenants receive these two orders. An important
distinction here, compared to the situation in Figure 2, is that the lieutenants
are aware that the commander is a traitor. This is because the commander's signature
appears on two different orders, and since only the commander could have generated
those signatures, the lieutenants can safely know that their commander is a traitor.

> Note that there are more solutions for different scenarios (i.e. the case where not
> all the lieutenants are directly connected to the commander) where a solution exists.
> I would refer to the formal [paper](https://lamport.azurewebsites.net/pubs/byz.pdf)
> in case you are interested in reading more about this subject. There are also systems,
> such as blockchain systems, that reach consensus in a more *novel* way while still
> being Byzantine fault tolerant.

---

### References
1. ACM Transactions on Programming Languages and Systems, Volume 4, Issue 3.
pp 382–401 https://doi.org/10.1145/357172.357176
2. Kleppmann, M. (2018) Designing data-intensive applications: The big ideas behind
reliable, scalable, and maintainable systems. Sebastopol, CA: O’Reilly Media. 
3. Narayanan, A. et al. (2016) ‘Chapter 2: How Bitcoin Achieves Decentralization’,
in Bitcoin and cryptocurrency technologies: A comprehensive introduction.
Princeton, NJ: Princeton University Press. 

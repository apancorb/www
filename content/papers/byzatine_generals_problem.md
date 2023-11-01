---
title: "The Byzantine Generals Problem"
author: ["Leslie Lamport", "Robert Shostak", "Marshall Pease"]
ShowReadingTime: false
cover:
    image: /papers/byzantine_generals_problem.png
    caption:
tags: ["distributed systems", "byzantine generals problem", "computer networks", "fault tolerance", "reliability"]
draft: false
---

The [paper](https://lamport.azurewebsites.net/pubs/byz.pdf) describes how reliable
computer systems must handle malfunctioning components that give conflicting
information to different parts of the system. This situation can be expressed
abstractly in terms of a group of generals of the Byzantine army camped with their
troops around an enemy city. Communicating only by messenger, the generals must agree
upon a common battle plan. However, one or more of them may be traitors who will
try to confuse the others. The problem is to find an algorithm to ensure that the
loyal generals will reach agreement. It is shown that, using only oral messages, 
this problem is solvable if and only if more than two-thirds of the generals
are loyal; so a single traitor can confound two loyal generals. With unforgeable
written messages, the problem is solvable for any number of generals and possible
traitors. Applications of the solutions to reliable computer systems are then discussed. 

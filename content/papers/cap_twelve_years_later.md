---
title: "CAP Twelve Years Later: How the “Rules” Have Changed"
author: "Eric Brewer"
ShowReadingTime: false
cover:
    image: /papers/cap_twelve_years_later.png
    caption: "[CAP Twelve Years Later: How the “Rules” Have Changed](https://ieeexplore.ieee.org/document/6133253)"
tags: ["cap", "distributed systems", "system design", "web", "databases"]
draft: false
---

In the decade since its introduction, designers and researchers have used (and
sometimes abused) the CAP theorem as a reason to explore a wide variety of novel
distributed systems. The NoSQL movement also has applied it as an argument against
traditional databases.

The CAP theorem states that any networked shared-data system can have at most two
of three desirable properties:

* consistency (C) equivalent to having a single up-to-date
copy of the data;
* high availability (A) of that data (for updates); and
* tolerance to network partitions (P).

This expression of CAP served its purpose, which was to open the minds of designers
to a wider range of systems and tradeoffs; indeed, in the past decade, a vast range
of new systems has emerged, as well as much debate on the relative merits of consistency
and availability. The “2 of 3” formulation was always misleading because it tended to
oversimplify the tensions among properties. Now such nuances matter. CAP prohibits
only a tiny part of the design space: perfect availability and consistency in the presence
of partitions, which are rare.

Although designers still need to choose between consistency and availability when
partitions are present, there is an incredible range of flexibility for handling
partitions and recovering from them. The modern CAP goal should be to maximize
combinations of consistency and availability that make sense for the specific
application. Such an approach incorporates plans for operation during a partition
and for recovery afterward, thus helping designers think about CAP beyond its
historically perceived limitations.

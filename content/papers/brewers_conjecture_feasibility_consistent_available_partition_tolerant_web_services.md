---
title: "Brewer's Conjunction and the Feasibility of Consistent, Available, Partition-Tolerant Web Services"
author: ["Seth Gilbert", "Nancy Lynch"]
ShowReadingTime: false
cover:
    image: /papers/brewers_conjecture_feasibility_consistent_available_partition_tolerant_web_services.png
    caption: "[Brewer's Conjunction and the Feasibility of Consistent, Available, Partition-Tolerant Web Services](https://dl.acm.org/doi/10.1145/564585.564601)"
tags: ["cap", "distributed systems", "system design", "databases", "web"]
draft: false
---

When designing distributed web services, there are three properties that are
commonly desired: consistency, availability, and partition tolerance. It is
impossible to achieve all three. In this note, we prove this conjecture in the
asynchronous network model, and then discuss solutions to this dilemma in the
partially synchronous model.

---
layout:  post
title:   When indices attack
date:    2019-03-04 16:00:00
summary: The hidden cost of indices in RDBMSes.
---

Let’s say you’re working on a code-base that’s been around for a number of years. A few generations of engineers have been working on it, and the tables in its database reflect that multitude of eras, approaches, and engineering personalities. As time goes on—new features are added and the service gains users—things start to _slow down_.

Adding more caching gets some immediate performance wins. But the observability and tracing tools reveal that one segment is unimproved—in fact it keeps on getting worse: transactional performance.

In write-heavy applications, such as e-commerce and financial, the speed of database transactions determine the limits of the service. A read-only e-commerce site can serve vast amounts of traffic, but a surprisingly small number of concurrent customers trying to actually buy something can bring it to a halt. If you’re willing to ferret through 2000s Internet archives you can find lots of engineering posts & talks from certain e-commerce giants talking about this problem.

The solution—for the smaller fish who aren’t giant corporations with many engineers—is simple: drop indices. Remember those generations of previous engineers? They probably added indices for queries that are no longer happening. And if they are still happening you can optimize them to reduce the number of indices they actually need.

An index is an additional piece of bookkeeping the database has to do when a record is changed. And although RDBMSes are impressively good at transactional bookkeeping, they are still bound by the limits of reality. On tables which are frequently read and written I’ve observed each additional index causing a linear (or worse!) increase on the latency of writes. Deleting a lot of old indices has resulted in orders-of-magnitude improvements in transactional latency. So if you’re seeing `UPDATE`s that you’d expect to take 2ms take 20ms, check your indices.

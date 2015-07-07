---
layout: post
title:  Problem-solving approaches and Instagram feeds
date:   2014-07-22 14:00:00
---

Last week I saw an interesting post (from a year ago) on the VMware vFabric blog on [how Instagram built their users' feeds](http://blogs.vmware.com/vfabric/2013/04/how-instagram-feeds-work-celery-and-rabbitmq.html). It's a pretty short post and is *definitely* worth a read. What is especially cool is it presents a classic type of computer science problem&mdash;we have some data that we need to store and later retrieve in a complex way&mdash;and different ways of approaching that problem. There's a lot to be learned from following the thinking about those approaches and learning about the implementation of a solution to that problem.

It's also worth thinking about the different aspects of their solution: individual FILO queues for each user to represent their feeds, with a central task queue to process insertions into those user feed queues. This is a really neat solution. However there's some fun follow-on problems with it. For example, what do you do when the user follows/unfollows someone? Their feed queue will have to be updated, of course; one potential solution to this new problem would be to create a new task to update their queue. That's a lot more complex a task compared to simple fan-out insertions, so I'd set this up as a separate queue with lower priority so that inserts&mdash;the most important operation in the system&mdash;continue to be as fast as possible.

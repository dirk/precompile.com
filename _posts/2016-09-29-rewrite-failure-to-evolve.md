---
layout:  post
title:   A rewrite is a failure to evolve
date:    2016-09-29 20:45:00
summary: Why one should almost always refactor instead.
---

Opinions are normal. Having one's opinions change is normal. The opinions you had when you started working on a system are probably different from those you have now. You've also learned things over the course of working on that system.

This grows with the number of people working on a single system. That diversity of views is not a bad thing. In fact, it helps ensure the system is constructed in as holistic a way possible.

### The proposition

At some point an enterprising person will say, "This system or sub-system is old and complex and so on. Let's rewrite it to be simpler and faster and so forth!" While understandable—I think this sometimes—it's most likely not a good idea.

The idea of a rewrite is an implicit proposition that the existing system is so unsustainable that the cost of replacing it is less than the cost of maintaining and improving it. This is almost never true.

#### Software is fighting to survive

Its users and maintainers place demands upon it: if it doesn't fulfill those demands then it is replaced. A complete rewrite implies that the entire system is not fulfilling demands. But is this true?

I've yet to see a rewrite proposed for a piece of software that was not performing its duties. If the software is doing its job then we should allow it to survive.

It is possible to maintain and improve old systems. This is not infrequent. We do it all the time when we change existing systems to add, change, or remove behavior.

### We are underestimaters

People are an ambitious lot. We are also eager to retry things: practice is built upon doing things better the second time around. This is a good behavior!

For small tasks we improve with repetition. But large systems are not small tasks, so we can't apply that principle to rewriting those systems. In doing so we underestimate the cost of our endeavor. And underestimating the cost of work is dangerous.

#### Short term pain for long term gain

A large, existing system is more intimidating than a large system that doesn't exist. We can't anticipate the "pain" of rewriting a large system because we haven't yet rewritten it. Because it is sitting in front of us we can gauge the difficulty of refactoring the existing system.

It is more intimidating and difficult to initially approach an existing system. But the upside is that the system already exists. The bugs/edge cases/etc. are already known. There aren't going to be surprises.

Over the years I've seen a lot of surprises during rewrites. Descriptions of those surprises generally start with, "So it turns out this was way harder than we thought..." Surprises are bad for business and make your job harder.

### Evolution is easy, creation is hard

The hardest work was already done to bring the system into existence from nothing. The system has proven it works and fulfills its demands. Killing the system is therefore misguided; instead you can make the system evolve.

As far as the actual doing of a rewrite, that's a topic left for a later time. I like to alternate between working and planning, letting issues that arrise during the work feed back into the planning. This helps with staying on course and avoiding getting bogged down in one particular problem. [Other people](https://rkoutnik.com/) have recommended [Working Effectively with Legacy Code][] by Michael C. Feathers.

[Working Effectively with Legacy Code]: https://www.amazon.com/Working-Effectively-Legacy-Michael-Feathers/dp/0131177052/ref=as_li_ss_tl?ie=UTF8&linkCode=ll1&tag=dirkto-20&linkId=89ae46866a5546d90565b85428ec985a

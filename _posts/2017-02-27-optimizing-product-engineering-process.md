---
layout:  post
title:   Optimizing product engineering process
date:    2017-02-27 18:45:00
summary: Principles for designing a lightweight and async process.
---

I won't prescribe any acronyms or branded methodologies for product engineering. They aren't nuanced, and nuance is necessary in a process because it must be able to adapt to the demands placed upon it.

Instead, I think there are two key principles that are critical in guiding process decisions: tight coupling and minimal shared state. These principles can then be applied to the choice of project management systems, version control & deployment strategy, meeting organization, and the like.

### Tight coupling

People who have participated in code review or architecture design with me will be familiar with my distaste for loose coupling: Einstein's phrase "spooky action at a distance" is commonly quoted. Tight communication between people shortens the feedback loops of the product development process. The longer it takes to get stakeholder approval, mocks/designs in hand, or the code reviewed, the longer it will take to reach the goal.

This doesn't mean the entire team needs to be all together. It means that each person should be closely tied to the people they rely upon. For example, project managers tend to end up as the central nodes: they have to communicate with stakeholders, designers, and engineers. In contrast engineers may need night coupling with the PM, designers, and their peers to quickly implement designs, author & review code, and deploy product.

This also doesn't mean communication must be in direct, high-bandwidth media. Team IM (Slack, if we're honest) is perfectly fine. (Also [use public channels][] for opt-in on-demand context sharing.) It's the content of the communication and the strength of the relationship that matters.

[use public channels]: https://twitter.com/drkgdsdn/status/834581347402264577

### Minimize shared state

Shared state is troublesome in software; it's also troublesome in process. More people involved in a thing means more state sharing is necessary. More tools being used means more state consistency must be maintained.

There's a reason distributed state is [an unsolved problem][] in computer science and software engineering. Therefore, avoid turning a process into a system of distributed state. Communication and coordination between two people is far easier than between ten people.

In practice this means keeping the number of people involved in a "thing" as small as possible (and be wary of scope & personnel creep on that thing). It also means being ruthless in pruning tools and steps out of a process.

[an unsolved problem]: https://en.wikipedia.org/wiki/Byzantine_fault_tolerance

#### Lightweight and async

Keep *all* tooling lightweight: reduce the clicks necessary in your PM software, reduce the time it takes to send a message, reduce the time it takes for building, running tests, and deploying.

Keep coordination as async as possible: prefer opt-in on-demand context sharing over meetings (remember what I said about team IM above?).

By keeping things lightweight and async you can move the tooling and process out of the way so that people can communicate closely and iterate rapidly.

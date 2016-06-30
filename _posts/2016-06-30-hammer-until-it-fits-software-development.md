---
layout:  post
title:   Hammer until it fits software development
date:    2016-06-30 12:15:00
summary: Develop healthier habits for incremental code reworking.
---

It's tough to build new features in the middle of large existing systems. There are a lot of moving parts, and for the sake of expediency we just stick our new parts in the middle. Of course that breaks other stuff, so we then hammer on the various parts until it all works again. This approach results in bad code, because we're not thinking holistically about how the new parts fit into the entire system.

### Avoiding unhealthy habits

It's easy to take this approach; I definitely do it myself. There are, however, two changes we can make in how we work to avoid falling into this behavior:

1. **Go just a little bit slower.** I routinely get caught up in the run-error-fix-repeat loop. That's not a healthy place to be in; you're not thinking about the whole code, instead you're just thinking about the single site of the error. By taking a moment to step back you can get a better view of the problem, and this can lead to a better solution.
2. **Don't be afraid to refactor.** We're hesitant to touch old code because we make the incorrect assumption that it's "gold" and works. We just want to get the feature shipped and not have to deal with anything else. But this sweeps the nuance and interconnected of the system under the rug. Willful ignorance does not result in a good outcome. Gradual refactoring in tandem with new development is essential to maintaining a healthy code-base.

A little bit of extra awareness of process goes a long way, as rushed work produces subpar results. Remember that unconsidered development devalues your thought & craft.

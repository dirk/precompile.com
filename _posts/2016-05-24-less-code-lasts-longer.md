---
layout:  post
title:   Less code lasts longer
date:    2016-05-24 19:45:00
summary: Maintenance is easier when you have less to maintain.
---

Remember March's npm [left-pad][] debacle? Take note that we all paid attention to the fact that it broke then. Nobody paid attention to how long it had been working.

It was published in March 2014; it was unpublished in March 2016. In the two years between those events it saw 1 feature commit, 1 patch commit, and 1 patch version release. That's 3 changes in 2 years for a package that, per npm's blog, thousands of other projects relied on.

That little amount of change no small feat for such a widely adopted package. It achieved that due to its size and simplicity, or rather lack thereof. More code means more risk of errors, and errors require changes to fix them.

[left-pad]: http://blog.npmjs.org/post/141577284765/kik-left-pad-and-npm

#### Beware the traps of terseness

When I say "less code", I don't mean less lines of code: I mean code that does less. A fast, convenient solution or a clever, terse solution are rarely the best ways to solve a problem. Both reduce immediate costs—time or lines of code—in exchange for greater maintenance costs down the road. Expediency and cleverness also often hide risk and complexity.

I believe in designing things that are simple and maintainable. They might not be the smallest things, and they're probably not the most elegant. But the simpler thing is easier to reason about, work with, and plays best with other things.

---
layout:  post
title:   Cleverness is the bane of maintainability
date:    2016-04-21 19:45:00
summary: Prefer simple and straightforward code over smart and fancy code.
---

Programmers like making clever contraptions out of code: we have [quines](https://en.wikipedia.org/wiki/Quine_(computing)), the [Underhanded C Contest](http://www.underhanded-c.org/), and [nerd snipe](https://xkcd.com/356/) each other often. These clever designs—though fun to make—can quickly become dangerous when introduced to a team environment and the demands of maintainability.

A clever design may be clean and obvious to its original author, but it's almost always bewildering and inscrutable to the reader. This is *very* easy to do in Ruby: with just a few fancy applications of duck typing and polymorphism you can end up with a system that is very difficult to pick apart and maintain. By creating abstraction and indirection in pursuit of clever designs you create additional steps for others to pick apart and hold in their mind.

### Repetition isn't always evil

"Don't repeat yourself" is a common mantra, but it's nowhere near universally applicable. It's tempting to create a novel abstraction to eliminate a few repetitions, but the hidden cost of that abstraction is that it will be much harder for a future reader to understand. Prefer some repetition over excessive abstraction or fancy constructs.

A thoughtful and balanced application of the [law of Demeter](https://en.wikipedia.org/wiki/Law_of_Demeter) and avoidance of cleverness will help you produce systems that are more straightforward and easier to understand. And that will pay dividends over time as others—or even yourself if you're as forgetful as me—work with that code.

### Design for maintainability

We want there to be as few steps or degrees of indirection, as that reduces the scope of what a programmer must keep in their mind when working with the system. Reading and writing code takes enough mental juggling, and maintainable (ie. good) code reduces the amount of juggling that the programmer has to do.

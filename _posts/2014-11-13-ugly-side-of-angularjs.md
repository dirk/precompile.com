---
layout: post
title:  Ugly side of AngularJS
date:   2014-11-13 17:00:00
---

Part of my work with Fetchnotes over the past half-of-a-year involved some brief excursions into coding in the land of AngularJS. Those were not the most pleasant of excursions: for the most part they were frustrating and unsettling. As a proponent of the old school traditions of pragmatic minimalism, Angular felt like a confusing forest of maximalist design and over-the-top reinventing of the wheel. I had **deep concerns** about the performance and maintainability issues caused by Angular's special (not in a good way) dependency injection and data binding systems.

As it turns out these concerns were not unfounded, and have been shared and corroborated by others, most notably Lars Eidnes' blog post about what they see as the ["bad parts" of AngularJS](http://larseidnes.com/2014/11/05/angularjs-the-bad-parts/). Reading it I found myself concurring with many of the conclusions Lars reached, these approaches and implementations are not good. Seeing as I could dedicate pages to the issues of building comprehensible parallel scoping systems (ie. parallel scoping shared between multiple contexts: DOM and JavaScript), I will instead just briefly touch on the matters of dependency injection and the performance problems associated with data binding.

### Dependency hell

Anyone who has had even a brief experience with type systems (or the internals of dependency resolvers) will probably groan when they hear about dependency injection. The resolution of a dependency—for example saying module A depends on module B—is trivial in the base case but grows *rapidly* in computational complexity when multiple interdependencies are introduced. For this reason Angular offloads all that responsibility to the package manager (commonly bower) and just uses a naive resolution system.

However, that doesn't solve much, because Angular also uses parameter names for specifying dependencies, which simply **doesn't work** with JavaScript minifiers and other frontend optimization tools. So they had to introduce an alternate form for specifying dependencies in their pseudo-constructors (which are another problem to be discussed another day) which actually results in one either needing to introduced yet another preprocessing step (who doesn't love long build times?) or manually double-define-and-position-match dependencies in their pseudo-constructors.

### Data binding

In the abstract, data binding seems *lovely*. Change a value in JavaScript and have it automatically change in the DOM, change a value in the DOM and have it automatically change in JavaScript, what's not to like? Well the browser engine probably doesn't like it. There's two major issues at hand here: interdependencies and polling.

#### Dependency hell, again

Bidirectional binding for a few primitive values (let's say, strings) aren't that bad. Just a few observers to keep track of everything and do the heavy lifting, no problem. However, as you build more complex structured values then you get into interdependency hell again. Changing a subvalue of a bigger value that's also associated to other values causes a **cascading flood** of updates across the value binding graph.

This is terrible for performance. In JavaScript it necessitates lots of traversing of the polymorphic object graph to find and update the correct properties/values; this isn't exactly fast in an dynamic language, and it's also pretty difficult for the just-in-time compiler to generate code for this sort of behavior. It's even worse for the DOM; we don't yet have a reliable way to block the layout engine from reflowing and rerendering, so this cascade of updates to the DOM creates an even bigger cascade of reflows and rerenders that bogs down the browser and wastes tons of processor time.

#### Polling

There's also the problem of making these observers actually observe. Every two-way binding requires an observer to watch for changes on those DOM elements. Then whenever one of those things changes it has to propagate all those changes through the JavaScript *and* the DOM. This is not a recipe for a nice CPU usage profile; you end up with lots of expensive checking going on every tick of the polling ("digest" in Angular terminology) loop.

### Use a needle instead of bazooka

The take-away from all this—I think—is that Angular and similar magic-binding mega-frameworks are just **too much**. There is just no need for these massive systems for most applications, especially given their terrible accessibility characteristics and that they are the effective antithesis of progressive enhancement & unobtrusive JavaScript. Build for your users, not for a fancy new-fangled framework.

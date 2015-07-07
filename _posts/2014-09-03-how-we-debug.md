---
layout: post
title:  How we debug
date:   2014-09-03 16:00:00
---

A month ago Russell Ivanovic wrote about the issue of debugging [stack traces in Objective-C and Swift](http://rustyshelf.org/2014/08/07/thoughts-on-swift-from-an-idiot/). His conclusion—which I agree with—was not encouraging. Objective-C/Swift stack traces are *terrible*. Go read the post if you want to get an idea of just how bad they are.

So what's the reason behind these awful stack traces? And what can we learn from them about stack traces and (dynamic language) runtimes? The reason—as you may have guessed—lies partly in Objective-C's (and Swift's) heritage: the Smalltalk messaging paradigm. And the user interface system—as Apple has built it—relies upon sending messages through the many private and public API layers of the view hierarchy. Now, I'm a big fan of Smalltalk and messaging passing. However, building proper messaging requires that those messages be treated as *truly first-class* in both the runtime and the debugger. And unfortunately that isn't the case. We can see from the stack traces that the debugger is doing an okay job of finding the symbols for the context of the message send site, however it's pretty much clueless about the receiver. This is the kind of thing that happens with heavy runtime dynamicism: it's often very hard to know what's going on.

However, I think that this is the sort of thing that can be improved with additional "smarts" in the compiler. Additional debugging symbols (eg. better hinting of the message receiver) and more aggressive binding of types to message send sites (ie. err on the side of being overzealous resolution of types) could really enhance the ability of the debugger to figure out exactly what's going on at all points in the stack.

As our compilers and runtimes keep on advancing, I think we'll—hopefully—see more stuff in this vein. More computation power and memory storage gives us additional headroom for describing the code that the machine is executing, and that additional information really comes in handy when you and the machine are trying to figure out what went awry.

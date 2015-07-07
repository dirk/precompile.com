---
layout: post
title:  Announcing Hummingbird
date:   2015-04-02 19:00:00
---

A couple days ago I made public the [repository for Hummingbird], a new language created by [Ryan Gonzalez] and myself, and today I'm proud to actually "announce" it. Over the past year or so I've had the pleasure of being able to dive into new languages like [Swift] & [Rust] and older languages like [OCaml]. My experiences with all of these languages inspired the notion of approaching a type-checked, compiled language from the side of the programmer rather than from the side of the machine.

The oldest compilers (and even some specialized ones today) were designed from the side of the machine: provide a friendly high-level abstraction on top of low-level instructions. C is over 40 years old, yet you can still rather easily match up individual lines in a C source file with their corresponding lines in assembly. Even with [vtables](https://en.wikipedia.org/wiki/Virtual_method_table) and bytecode execution the same line-to-line correspondence can still be found in C++, Java, Python, and so forth.

Hummingbird was—and is—designed from the other direction. Rather than considering any eventual compilation target (in that regard we chose JavaScript as the first target because it was *very* low-hanging fruit), we started by looking at what we liked in our experiences with all these languages and platforms. A few things stood out: firstly, the C-style syntax of braces and such is nice, however it can get overbearing. We took inspiration from Swift in this and started looking for places to trim down the language while still keeping it fairly syntactically rigid and explicit.

Secondly, a well-defined yet simple syntax hierarchy is nice to have. A clear, straightforward structuring of blocks, statements, and expressions makes for confident authoring and easy comprehension.

Thirdly, a powerful type-system is one of the greatest tools a programmer can have. In this we took a great deal of inspiration from Rust and OCaml/ML. It's still very, very much a work in progress. But we're confident that Hummingbird's evolving type-system is going to be one of the friendliest out there. Type-checking is an immensely powerful tool at every stage of software's lifecycle: it aids in reasoning and guiding initial development, provides incredibly useful guarantees and assurances during testing, and makes reading of source code easier for those new to the code or those taking up the critical task of refactoring.

I'm very excited about the progress made so far on [Hummingbird], and look forward to the all the opportunities for innovation yet remaining. So go [check it out][Hummingbird]!

[Ryan Gonzalez]: http://ryngonzalez.com/
[repository for Hummingbird]: https://github.com/dirk/hummingbird
[Hummingbird]: http://hummingbird-lang.org/
[Swift]: https://developer.apple.com/swift/
[Rust]: http://www.rust-lang.org/
[OCaml]: https://ocaml.org/


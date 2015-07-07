---
layout: post
title:  Lingua JavaScript'a
date:   2014-11-26 21:00:00
---

The way that interactive experiences have been powered on the web has fundamentally changed over the past decade. JavaScript has gone from a little tool for adding extra bits of dynamic interactivity—remember [DHTML](http://en.wikipedia.org/wiki/Dynamic_HTML)?—to a nigh-ubuitious standard. The most remarkable thing about this spread of JavaScript is it's ubiquity in the context of the history of *execution engines*.

In the earlier ages of computing execution engines were just the raw processors that the native code ran upon. Then, as languages and methodologies progressed, came the introduction of interpreted and more dynamic languages. However, these also required an execution engine *in addition to* the native execution engine. This multi-tiered approach carried with it a significant performance penalty. To mitigate this [just-in-time compiling](http://en.wikipedia.org/wiki/Just-in-time_compilation) (JIT) interpreters were developed to speed up the execution of these languages. However, making a fast JIT is a **massive undertaking**.

JavaScript has seen an exceedingly widespread adoption, multiple execution engine implementors (ie. browser vendors), and little change in the core of the language, which makes it easier to implementors to focus on fine tuning their engines rather than adjusting to dramatic changes.

The result of this is some truly amazing performance in JavaScript performance these days. This also puts JavaScript in the unusual position of being a new target. Traditionally targets have been native machine code—for example x86 or ARM—but the incredible performance of modern JavaScript JIT-compiling interpreters means that one can write (or generate) JavaScript code that will perform reasonably close to native code.

What's neat about this is that JavaScript is **way easier** to generate than native code. Unlike traditional low-level instructions, JavaScript is a modern, high-level language. Even the more low-level extension to JavaScript, [asm.js](http://ejohn.org/blog/asmjs-javascript-compile-target/), is still friendly when compared to old-school assembly. JavaScript therefore represents a burgeoning new common execution engine for developing languages. Consider the explosion of languages featuring JavaScript as a compilation target: AtScript, ClojureScript, CoffeeScript, Dart, Haxe, Traceur, and TypeScript (many of these have JavaScript as their *only* target, and Traceur is a Javascript-to-Javascript compiler).

So if you're thinking about developing a new language or just fooling around with compilers: rather than dealing with complex native machine code generation, consider targeting plain old JavaScript!

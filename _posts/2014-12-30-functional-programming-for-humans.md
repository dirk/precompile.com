---
layout: post
title:  Functional programming for normal humans
date:   2014-12-30 16:00:00
---

Yesterday my formerly-of-Fetchnotes pal Giles Van Gruisen published a cool piece about an [infix pipeline operator] he'd come up with in Swift. This syntax extension allows one to go from a traditional nested format (or long, verbose sequential format that Go coders may be familiar with) for safely handling optionals to a concise, sequential format:

```swift
// Traditional nesting for handling optionals
if let b = OptionalProducer(a) {
  if let c = AnotherOptionalProducer(b) {
    DoSomething(c)
  }
}

// Pipeline of optionals
OptionalProducer(a)
  --> { AnotherOptionalProducer($0) }
  --> { DoSomething($0) }
```

[infix pipeline operator]: http://gilesvangruisen.com/writing-a-pipeline-operator-in-swift/

This is syntax makes the code much more clear and readable. Interestingly it's also pretty similar to the [`do`-notation](http://learnyouahaskell.com/a-fistful-of-monads#do-notation) of Haskell monads. Under the hood, Giles' Swift pipelines and Haskell monads are both utilizing the concept of constructing chains of function applications to handle impure operation (ie. because of external state some or all of the functions in that chain may fail and therefore produce `nil` or other kind of bad data).

However, compared to the complexities of Haskell I think that the Swift pipeline is much more understandable and usable for the basic, day-to-day tasks of creating a website, iOS/Mac application, and so forth. It uses a simple yet powerful syntax extension to enable you to write more clean, concise code. It's also a an application of basic functional programming concepts to real world tasks, which for the common programming tasks of normal humans is much more relevant than abstract, complex functional theory and esoteric syntaxes/constructs. Hopefully in the future we'll see even more instances of people utilizing the new crop of powerful compilers to come up with cool utilities like the pipeline operator.

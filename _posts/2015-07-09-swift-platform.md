---
layout: post
title:  Towards a platform for Swift
date:   2015-07-09 15:00:00
summary: A first step on the road to programming Swift without needing Xcode.
---

This year's WWDC saw Apple announce the coming of the [open-sourcing of and Linux compatibility for](https://developer.apple.com/swift/blog/?id=29) Swift. A fun and thoughtfully-designed language, Swift is a truly modern language that shows promise beyond just the land of Mac and iOS applications.

However, Swift is currently only usable through Xcode and its tooling—the IDE, Playgrounds, and the `xcodebuild` command-line interface. There are command-line REPL and compiler program interfaces—`swift` and `swiftc` respectively—however the latter, like most compilers, is ungainly to use directly and requires some verbose magic to compile any sort of non-trivial project structure.

#### We need a platform

Languages need platforms and ecosystems to make them useful. Haskell has [Cabal](https://www.haskell.org/cabal/), Rust has [Cargo](http://doc.crates.io/), C has make[^1], but right now Swift has just a couple package managers, [CocoaPods](https://cocoapods.org/) and [Carthage](https://github.com/Carthage/Carthage).

Compiled languages need more than just a package manager. Both Carthage and CocoaPods invoke the [Xcode build system](https://developer.apple.com/library/mac/documentation/Darwin/Reference/ManPages/man1/xcodebuild.1.html) under the hood, they can't build anything on their own. For Swift to be usable on Linux and in roles beyond building user applications it needs a system capable of building a Swift executable or library without the presence of Xcode. Most servers run Linux/BSD/etc., and web developers don't want to have to fire up the entire Xcode toolchain just to produce a binary. Furthermore they don't want their development system, their continuous integration system, or their web server to have to run Mac OS X and Xcode.

If Swift really is coming to Linux, then it needs a full-featured package manager and build system: something that can handle resolving dependencies, fetching those dependencies, and building the entire scope of a project and its dependencies. Xcode-on-the-Mac or some stripped-down multi-platform relative that's able to build `.xcodeproj` files[^2] is simply not going to cut it.

[^1]: And a lot of weird system magic.

[^2]: I feel daring even suggesting Apple would go that far.

### Enter Roost

[Roost](https://github.com/dirk/Roost) is an alpha-stage package manager and build system for Swift. It is heavily inspired by the syntax and operation of Rust's Cargo, and it aims to fill this gap in the ecosystem. In theory, the minute Swift binaries can be deployed to non-Mac OS X platforms (eg. web servers), Roost will be there and ready to build on those platforms.

As the language, toolchain, and interoperability improve there are a multitude of different applications for Swift: graphical applications, web applications, servers, etc. From reliable servers ([ARC](https://en.wikipedia.org/wiki/Automatic_Reference_Counting) and [optionals](https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/TheBasics.html#//apple_ref/doc/uid/TP40014097-CH5-ID330) are a powerful combination) to GUI applications, there are all sorts of possibilities, and Roost will hopefully be a powerful tool for building those various programs and libraries.

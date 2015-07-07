---
layout: post
title:  Address space layout randomization in FreeBSD
date:   2014-07-16 15:00:00
---

An [address space layout randomization (ASLR) RFC](http://lists.freebsd.org/pipermail/freebsd-arch/2014-July/015548.html) was recently posted on the [freebsd-arch](http://lists.freebsd.org/mailman/listinfo/freebsd-arch) mailing list, which is cause for excitement. ASLR is an important technology in system security: the randomization of the arrangement of data in a program's address space makes it more difficult for memory attacks&mdash;such as buffer overflow attacks&mdash;to access critical points in memory. Given the wide use of FreeBSD in many critical applications this is a *good thing*.

The RFC itself is also worth reading, as it covers the history, development, and implementation details of a specific deployment of ASLR. Implementing ASLR is no easy feat&mdash;requiring detailed coordination between the operating system kernel and specialized executables&mdash;so one can learn a lot from reading the developers' in-depth summary of their work.

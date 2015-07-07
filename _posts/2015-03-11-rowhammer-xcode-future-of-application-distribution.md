---
layout: post
title:  "Rowhammer, Xcode, and the future of application distribution"
date:   2015-03-11 16:00:00
---

The past week has seen some notable developments in application security. First there were the revelations [published by The Intercept](https://firstlook.org/theintercept/2015/03/10/ispy-cia-campaign-steal-apples-secrets/) (additional [analysis by Craigh Hockenberry](http://furbo.org/2015/03/10/xcode-compromised/)) of the CIA and Lockheed Martin stealing and/or cracking keys in Microsoft's BitLocker and Apple's signing keys embedded on the systems-on-a-chip powering their phones. Furthermore the CIA and their co-conspirators allegedly shipped malicious versions of Xcode which embedded the following in applications compiled with it: the developer's private keys, remote backdoors, and operating system & hardware security-disabling malware.

Additionally the [Google Project Zero security team revealed](http://googleprojectzero.blogspot.com/2015/03/exploiting-dram-rowhammer-bug-to-gain.html) flaw in modern DRAM modules that allows for privilege escalation and sandbox escape. The exploit&mdash;dubbed rowhammer&mdash;uses spontaneous bit-flipping induced by repeated row signaling to escape the operating system's memory security protections.

### Securing binaries

What is most interesting about these attacks&mdash;especially the ones [on Xcode and its compiler tools](http://furbo.org/2015/03/10/xcode-compromised/)&mdash;is how they focus on the distributor rather than the end user. The majority of past attacks&mdash;by both nations and third parties&mdash;have followed the strategy of exploiting known (potentially zero-day) vulnerabilities in programs running on the end user target (ie. flaws in browsers, plug-ins, services, and so forth).

Although [compiler-level attacks are decades old](https://www.schneier.com/blog/archives/2006/01/countering_trus.html), the renewed focus by state attackers with vast resources puts the focus back on compilers and the binaries they produce. Although the recent, and much overdue, widespread introduction of signed binaries in Windows and Mac has helped prevent the spread of malicious binaries, signing is not much use if the compiler tools themselves are producing malicious binaries.

#### Trusting compilers

The chicken-and-egg problem of evil compilers is certainly annoying, however&mdash;for very popular languages like C and C++&mdash;it has been sort of solved: just use multiple different compilers and compare their outputs. It's not a perfect solution by any means, but it does give some reassurance.

#### New approach to distribution

I also think these exploits&mdash;especially if they increase in popularity&mdash;point to a continuation of the shift away from distributing native binaries to low-level intermediate representations (IRs). This has existed for a long time in [.NET/CLI](http://en.wikipedia.org/wiki/Common_Intermediate_Language), Java, and Android. Intermediate representations offer a slightly higher-level abstraction above machine code, giving the benefit of easy multiarchitecture/multiplatform support. Additionally, since the IRs are processed by the end user, we have the opportunity to embed **low overhead analysis and verification** on the end user's system. Runtime inspection at the end-user level offers an incredible opportunity for monitoring and preventing malicious activity.

The privacy/permissions controls introduced by iOS (and to a lesser degree Android) are a nice step forward in protecting users. However, empowering the user's system to actively & deeply analyze the execution of programs opens up a whole new frontier of security. Yes, these kinds of capabilities have existed for a while in specialized, high-security Linux distributions and the like, but they carry not-insignificant penalties to performance and usability. Switching to well-designed intermediate representations with intelligent runtimes on the end user's system allows for this sort of protection without undue penalties to performance and usability. An smart runtime could potentially detect & thwart malicious backdoors/exfiltrations, prevent rowhammer-like exploits[^1], **and** continually optimize programs. Such runtime optimization would mean that software updates might *actually* make your system run faster!

All of these parts already exist separately. Ahead-of-time/just-in-time runtime compilers&mdash;from Java to .NET/Mono&mdash;have existed for a while and are constantly improving in performance. There is a wealth of knowledge in runtime analysis already existing in tools like Dtrace and Valgrind. We just need to bring all of these together to **build a better runtime**.

[^1]: For example, preventing a rowhammer-type exploit in a JavaScript runtime would simply require checking if the current machine is known to be vulnerable and introducing artificial latency into certain operations (such as highly-repetetive read-writes of an [ArrayBuffer](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/ArrayBuffer)).

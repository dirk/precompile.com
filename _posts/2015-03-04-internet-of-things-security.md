---
layout: post
title:  Security and the Internet of things
date:   2015-03-05 16:00:00
---

Yesterday [Ars Technica published a piece](http://arstechnica.com/security/2015/03/more-iot-insecurity-this-blu-ray-disc-pwns-pcs-and-dvd-players/) about a newly-revealed "One Disc"[^1]  that lumps together a variety of vulnerabilities to launch malicious executables on virtually any player platform. (See the original [NCC Group announcement](https://www.nccgroup.com/en/blog/2015/02/abusing-blu-ray-players-pt-1-sandbox-escapes/) for more in-depth details.)

The disc exploits a variety of weak points in different players'--from the little player in your home media center to one running on your computer--implementations of the Blu-ray Disc Java specification. (Yes, you read that right, *every Blu-Ray player uses Java ME* to provide its interactive features.) The exploit suite detects the player it's running on and then uses the correct vulnerability to break through the player sandbox & launch a corresponding platform-specific malicious executable. On a device the user would be completely unaware that this malicious code was running on their device with full access to the network, storage, and so forth. And on a desktop the malicious code would start out running at the user's permissions level with free reign over their user-space.

For the time being, **avoid Blu-ray discs**, and (if you haven't already) **disable AutoPlay** on Windows[^2].

These vulnerabilities are just another plop on the ever-growing turd-pile of security in the nascent Internet of things. Securing software and systems is hard, and it's *even harder* when you have to deal with sandboxed arbitrary code execution (ie. Flash, Java, JavaScript, etc.) and network access. Networking code is notorious for having security flaws (a quick search turns up decades of vulnerabilities in implementations of TCP, SSL, network application services, and so forth), and application sandboxes are legendary for their vulnerabilities (drive-by Flash and Java attacks are the bread and butter of end-user attacks on the web).

As we integrate more and more devices into our lives, we need to take a long, hard look at the value they provide in the relation to the quite-large risks their myriad security flaws currently pose. Is a "smart" Blu-ray player really worth it if the next disc you play is going install spyware on your player to track your every action? (And that's the good case; worse would be it you turning your player into a member of a botnet!)

[^1]: The boring younger sibling of the One Ring.
[^2]: AutoPlay, Internet Explorer, and ActiveX: the Notorious Three of seemingly-endless vulnerabilities in Windows.

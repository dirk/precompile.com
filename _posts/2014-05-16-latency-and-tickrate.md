---
layout: post
title:  Latency and tickrate
date:   2014-05-16 21:00:00
---

Following up from the latency and framerate post earlier this week, today I noticed an interesting fact about the much-troubled Battlefield 4. To say it's had a rocky year would be a gross understatement. It's not a good thing to have your release game feel like a beta, and that's exactly how Battlefield 4 has felt.

One of my biggest peeves with Battlefield 4, and its predecessor Battlefield 3, were their unresponsive multiplayer experience. Acts in comparable multiplayer games that would feel crisp and immediate instead felt like wading through molasses—or in some cases fail to even register with the server. This was on connections to servers with pings around 40 milliseconds!

Well, it turns out the developers finally are addressing it, and one of the largest culprits they've identified is a 10 Hz server refresh rate, aka. tickrate (via [Kotaku](http://kotaku.com/battlefield-4-is-finally-getting-fixed-1577452991)). A 10 Hz refresh rate means your latency can be as much as 100 milliseconds on top of input device latency, computer hardware latency, and network latency. And that 100 milliseconds really shows in the experience of the game: sluggish, awkward, and buggy. However, this does make for a teaching moment: when building interfaces for users, response times in all parts of your system—even if those parts are on a server hundreds of miles away from the user—matter.

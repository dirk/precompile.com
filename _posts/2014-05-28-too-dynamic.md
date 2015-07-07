---
layout: post
title:  Too dynamic
date:   2014-05-28 19:00:00
---

In an increasing drive to increase "engagement" or whatever the marketing people are calling it, sites are featuring more and more dynamic content: layered (ie. "parallax") DOM elements, complex typography, native and JS-driven animations, and *video*. This is bad for users.

Let's think about the big trends in computing these days: skyrocketing use of mobile devices, the proliferation of tablets, and the near-ubiquity of laptops. All of these devices rely on *batteries*. A sixth-sense awareness of the remaining charge on one's device(s) has become natural to most people.

These devices are also largely mobile, and trying to deliver this sort of large content—especially video—on low-bandwidth high-latency mobile connections leads to terrible user experience. When I'm on a slow, intermittent cellular data connection I *do not* want to download massive transparent PNGs and autoplaying background videos for your "engaging experience". That's not just bad user experience; it's downright disrespectful.

This doesn't factor in the energy cost of rendering fancy web fonts, multi-layer transparent DOM elements, and video elements. Requiring a mobile user to play a video—especially if it's autoplaying (let's make this categorical, actually: **never, ever autoplay videos**)—is akin to walking up to them, ripping the battery out of their device, smashing it with a sledgehammer, and then urinating on its remains for good measure. Demanding video playback from your users is the new popup ad.

Here's my level of engagement when you autoplay or ask/require me to play a video: *close tab*.

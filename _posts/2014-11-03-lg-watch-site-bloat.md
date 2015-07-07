---
layout: post
title:  LG Watch site's bad bloat
date:   2014-11-03 15:00:00
---

Sverre Nøkleby recently blogged about the [serious bloat on the LG G Watch](http://perf.fail/post/101500047374/lg-g-watch-site-delivers-an-images-gone-wrong-54mb-rwd), and it's bad, it's really bad. Do check out their blog post; it has some nice details on the inane stuff the site is doing.

Visiting the page causes one's browser to download **54 megabytes of assets**. This is outrageous: the total asset payload comes out to over 300 different image requests, 6 stylesheet requests, and 17 script requests. And the result is not impressive at all; in fact it's [pretty cluttered and ugly](https://twitter.com/Esherido/status/528997423729737728).

This is not where responsive web design should be taking us. The existence of all these device sizes and display densities is not an opportunity to go wild with every image and stylesheet and CSS3/JS feature one can think of. A good experience is not the most shiny and animated experience, a good experience is one that communicates effectively to the user and enables the user to get what they're trying to do done.

This is bad user experience, plain and simple. Even a desktop/laptop on a residential network connection is not going to provide a good experience with a 50 megabyte payload. On a fairly fast network connection—advertised by the ISP as 30 megabits-per-second down—this payload size results in a page load time of over 12 seconds. On my parents' slower connection this site would take **over 4 minutes to load**, and doing anything else on the Internet at the same time would be nigh-impossible during those 4-plus minutes. To put it bluntly: don't do this, people.

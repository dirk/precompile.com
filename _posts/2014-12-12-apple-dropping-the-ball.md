---
layout: post
title:  Apple's dropping the ball
date:   2014-12-12 18:00:00
---

Yesterday morning at around 9 AM I began updating my computer to OS X Yosemite. That update completed this morning at around 10 AM. That is a **25 hour update**... on an Apple product. Updating an operating system on a spinning disk is definitely an overnight sort of operation, but an incremental update to a reasonably well-engineering system running on a fast solid state drive? I think that should take no more than an hour, right?

A twenty-five hour update is the sort of thing you would expect from Microsoft Windows on a crappy spinning disk in the 1990s, **not** Mac OS X on solid state hardware in 2014. That's just ridiculous. In fact, I vaguely remember updating Windows in the 90s and 2000s taking just an afternoon at the most. Sure, sometimes you'd start it when you went to bed, but if you got up in the night to use the restroom you'd find it had finished updating long ago.

The cause of this, as I discovered through some investigation of my own and tweets from [some](https://twitter.com/bwjacobs/status/543193833933393921) [friends](https://twitter.com/amblin/status/543228415768150018) is quite possibly the [slowest directory merge] in the history of file systems being performed on the `/usr/local` directory (and a few other directories). Now, I checked that directory on my machine, and it has approximately 1.8 million files in it. Since I work on a lot of different things with widely varying dependencies, mine is quite big. However, more modestly-sized `/usr/local` directories are still causing massive hiccups in the upgrade process for developers.

[slowest directory merge]: https://jimlindley.com/blog/yosemite-upgrade-homebrew-tips/#symptoms

This shouldn't be happening; merging directories isn't hard. In fact, if any Apple engineers happen upon this post, [here's a handy reference](http://lmgtfy.com/?q=unix+merge+directories) for how to do so with Unix tools from the 1970s. You don't need to reinvent the wheel, especially if that wheel is farcically slow.

Even more saddening is that this is just another stumble in an increasingly long litany of stumbles by Apple, from the [iOS 8 upgrade problems](http://bits.blogs.nytimes.com/2014/09/24/apple-pulls-software-update-after-iphone-problems/) to the [other Yosemite upgrade problems](http://fieldguide.gizmodo.com/the-worst-bugs-in-os-x-yosemite-and-how-to-fix-them-1652690924) to the [continually broken Gmail support in Apple Mail](http://9to5mac.com/2014/02/26/even-after-os-x-10-9-2-mavericks-update-users-still-complaining-about-mail-issues/). These issues are indicative of the ongoing problem within Apple of not having sufficient engineering capacity to adequately work on and maintain its ever-growing range of products and services. And these stumbles will continue unless Apple can figure out--through hiring, management, quality assurance, and the like--how to address this underlying capacity problem.

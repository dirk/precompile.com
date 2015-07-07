---
layout: post
title:  Keep the URLs visible
---

Recently, the bleeding-edge Chrome Canary shipped with a feature that essentially removed the URL. The location field of the browser became merely a Google search field. Much has already been said about this; specifically, [Allen Pike](http://www.allenpike.com/2014/burying-the-url/) has some very good thoughts on the issue.

I'm strongly in the keep-the-URL camp for a variety of reasons: sentimentality, love of the low-level, and *security*. Security is the most important reason for keeping these clutzy strings of weird characters. The purpose of the URL is to describe a *uniform location for a resource*. Although they are confusing to some users and their usability is far from ideal, they still serve that integral role for being a standardized, trusted way of identifying where one is in the vast web of the Internet.

The URL simply communicates far too much to be left out. It tells us about the security of our connection and it tells us what exactly we are connecting to. As more and more of our critical infrastructure goes online, knowing these basic aspects of our navigation is imperative. Assuming there's no active man-in-the-middle attack, we can trust the URL to be the exact, canonical description of what we're connecting and how we're connecting to it. DNS and the PKI are many-party and relatively-open systems; Google is neither of those. I think it's safe to say that we can trust DNS and X.509 PKI far more than we can trust Google or any other single vendor. URLs depend on open technology; let's keep the web open, let's keep URLs.

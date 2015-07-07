---
layout: post
title:  Let's talk frontend performance
date:   2014-11-05 15:00:00
---

Building a compelling frontend experience without JavaScript is pretty hard these days. Users expect interfaces that respond near-immediately to their interactions. We're also building complex very complex, feature rich sites and applications on the web.

Multiply these big sets of features by the demands of rich interactive experiences and the result is **a lot of code being required** to make all that possible. However, lots of code requires lots of bandwidth and computational power. Even with the advances of microprocessor and mobile network technologies, we're still heavily bounded by the performance of the devices our experiences run on and the networks they're delivered on.

Building and delivering a massive JavaScript payload to the user's browser is not a recipe for that user having a good experience. On a laptop or phone that big chunk of code (and stylesheets and images) is going to **severely effect** their device—sucking up precious battery power to download those resources over their cellular connections, render complex stylesheets and markup, and parse & execute large amounts of JavaScript. (Think that those basic parsing and initialization routines don't take up resources? Think again. Tim Kadlec did a nice little [investigation of JavaScript parsing and execution times](http://timkadlec.com/2014/09/js-parse-and-execution-time/), and the results should definitely make you think twice before adding another jQuery plugin or other frontend library.)

This is not conducive to a good user experience at all, and for this reason we're starting to see laudable efforts by developers to optimize and streamline the way they're building these rich experiences. I hope that this marks the decline of the giant frontend JavaScript framework—Angular, Backbone, Ember, and so forth—and the return of small, streamlined frontend JavaScript design patterns.

One of the best examples of what I hope-will-become-a-trend of streamlining is [Shopify's recent rewrite of their frontend architecture](http://www.shopify.com/technology/15646068-rebuilding-the-shopify-admin-improving-developer-productivity-by-deleting-28-000-lines-of-javascript). In total the Shopify team cut 28,000 lines of JavaScript, 4,000 lines of Ruby, and trimmed a total of 2.5 megabytes out of the JavaScript payload (from 4.1 to 1.6 MB)! Their post on the methodology behind this big rewrite is very much worth a read. It's time for us all to take a good long look at how we're structuring our frontend code to find places to really optimize and remove bloat and inefficiencies.

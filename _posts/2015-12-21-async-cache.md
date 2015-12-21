---
layout:  post
title:   Async caching
date:    2015-12-21 16:00:00
summary: Serve slightly-stale data to maintain responsiveness.
---

We serve a pretty high volume of traffic at [Everlane](https://www.everlane.com/), and our content and inventory can change frequently: content managers introduce new experiences and products' availabilities change as they are purchased.

The above means that we have to rebuild the total, expansive "state" of data associated with pages on our site many times per day. This rebuilding only happens when necessary: that is, when a user requests that resource. That rebuilding can take a few seconds or longer: which is a really crappy for the user who's trying to browse our site. To avoid that blocking we built an asynchronous caching system.

### Strategy

The idea behind async caching is to serve a slightly stale version of the resource to the user immediately, while doing the actual regeneration of the up-to-date version in the background (we use [Sidekiq](http://sidekiq.org/) workers).

This required a deviation from the standard Ruby on Rails caching practices. Rather than storing this timestamp in the cache key, we moved it into a separate piece of metadata associated with the cache (we decided to call this the *version*). A cache entry therefore became a [3-tuple of a locator/key, version, and the data](https://github.com/Everlane/async_cache#cache-structure). The locator would stay constant, while the version would allow us to determine if the entry was out-of-date and needed to be regenerated.

### The results

We have almost entirely prevented[^1] these long requests from impacting our end users. And, because the implementation of async caching requires using an [intermediate view-model class], we've been able to start separating our API tests to focus separately on the structure (what *fields* are in a response?) and the semantics (what does an action *do*?) of our API endpoints.

[intermediate view-model class]: https://github.com/Everlane/async_cache/blob/c56f7ce7/examples/examples_controller.rb#L30-L42

Async caching definitely isn't the most straightforward of caching systems, but appropriately used it can make a big difference. I encourage checking out the [`aynsc_cache` gem on GitHub](https://github.com/Everlane/async_cache) for more details and examples.

[^1]: The one case of slowness that we cannot avoid is when a user visits an *entirely* uncached resource. When this happens we *do* have to block the response while generating the data.

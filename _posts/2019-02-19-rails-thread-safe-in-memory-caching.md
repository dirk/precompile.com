---
layout:  post
title:   Thread-safe in-memory caching in Rails
date:    2019-02-19 02:45:00
summary: A trivial cache design to avoid (de)serialization costs.
---

Ruby on Rails has good [out-of-the-box caching][]. The vast majority of the time it is all you need: it reliably works in controllers, domain logic, models (though I'd discourage it), and most importantly views.

[out-of-the-box caching]: https://guides.rubyonrails.org/caching_with_rails.html

However, Rails' caching can slow down if you have to call the cache frequently (memoize!) or cache a large value. The latter can become very slow, especially a large set of interrelated ActiveRecord model instances. Recently I observed the serialization and deserialization times for a tree of around 1,000 model instances were an *order of magnitude greater* than the round trip time to the cache: the cache took <1ms, but deserializing the tree of object instances took over 100ms.

### Keep it in memory

The solution is to keep the objects cached in memory on the Ruby heap. That way you never need to pay (de)serialization costs. Rails provides a [`Module.thread_mattr_accessor` method](https://api.rubyonrails.org/classes/Module.html#method-i-thread_mattr_accessor) which makes it easy to build a trivial thread-safe cache service.

```rb
module ValueCache
  extend self

  thread_mattr_accessor :cached_value

  def value
    if cached_value && !expired?
      return cached_value
    end

    self.cached_value = compute_value

    cached_value
  end
end
```

Rails' implementation ensures every thread has its own storage location for `cached_value`; therefore this should be thread-safe (assuming `#compute_value` can safely run both concurrently and in parallel). And that assertion is supported by the fact that I haven't observed any unsafe behavior in production.

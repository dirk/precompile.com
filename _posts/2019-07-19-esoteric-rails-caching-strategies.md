---
layout:  post
title:   Esoteric Rails caching strategies
date:    2019-07-19 3:15:00
summary: For when Rails.fetch isn't enough.
---

The out-of-the-box strategy provided by Rails’ `#fetch` method does the job for the majority of situations in an application. However, there are some situations where it doesn’t.

### Unreliable services in the critical path

Sometimes you have to query a remote service in a critical path, such as rendering the page. An example of that is an A/B testing service which provides a JSON blob of configuration describing the tests to be run on the page. In order to render the page you need that JSON blob.

Rails’ standard strategy won’t work here because it makes the rendering of _your_ page dependent on the availability and latency of the remote service providing that JSON blob. If it’s slow or unavailable when the cache expires then your page rendering is going to suddenly be slow or unavailable!

The Rails-interface-compatible solution to this is what I’ve been calling a “selfish cache” (I couldn’t find any existing public examples of this, so I had to come up with the name myself). The implementation in Ruby is less than a page:

```rb
def selfish_fetch(key, expires_in:)
  must_expire_in = expires_in * 2
	entry = Rails.cache.read(key, expires_in: must_expire_in)
	expired = entry.nil? || entry.expired?

  if expired
    begin
      value = yield
      Rails.cache.write(
        key,
        SelfishEntry.new(value, expires_in),
        expires_in: must_expire_in,
      )
      value
    rescue => error
      # Reraise if there's nothing to fall back to.
      raise if entry.nil?
      backend.write(key, entry.reset, expires_in: must_expire_in)
      # Report to your exception handling here.
      # Then return the previous value:
      entry.value
    end
  else
    entry.value
  end
end

class SelfishEntry
  attr_reader :value

  def initialize(value, expires_in)
    @value = value
    @created_at = Time.now.to_f
    @expires_in = expires_in.to_f
  end

  def expired?
    @expires_in && @created_at + @expires_in <= Time.now.to_f
  end

  def reset
    self.class.new(value, @expires_in)
  end
end
```

### Extremely slow recalculation

If recalculating the value is extremely expensive _and_ it’s okay to have stale data: then return the stale data and recalculate in the background. That way the requester gets a fast, cached response, and the cache is kept made consistent with the source asynchronously. Implementing this is a bit more complex than the selfish cache, so over the course of my work I built the [`async_cache` Ruby gem](https://github.com/BrandlessInc/async_cache) to provide this functionality.

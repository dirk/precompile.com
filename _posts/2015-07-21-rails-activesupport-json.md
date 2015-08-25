---
layout:  post
title:   Using Oj in Rails' ActiveSupport JSON
date:    2015-07-25 16:00:00
summary: Making all of your JSON serialization be fast.
---

Recently at [Everlane] I ran into some interesting performance quirks with how we handled JSON serialization: wherever we called `.to_json` on something—even a small something—things got **really** slow.

We have the [Oj] gem in use for JSON (de)serialization, so I thought things *should* be fast, but after some digging it turned out that the [`.to_json` method on objects](https://github.com/rails/rails/blob/master/activesupport/lib/active_support/core_ext/object/json.rb#L37) still used the [plain old JSON gem through `ActiveSupport::JSON::Encoding`](https://github.com/rails/rails/blob/master/activesupport/lib/active_support/json/encoding.rb#L123).

Thankfully, it's easy to switch out that encoder for a faster one:

```ruby
# config/initializers/json_encoding.rb
module ActiveSupport::JSON::Encoding
  class Oj < JSONGemEncoder
    def encode value
      ::Oj.dump(value.as_json options.dup)
    end
  end
end

ActiveSupport.json_encoder = ActiveSupport::JSON::Encoding::Oj
```

Now go forth and have all your JSON encoding be swift!

[Everlane]: https://www.everlane.com
[Oj]: https://github.com/ohler55/oj

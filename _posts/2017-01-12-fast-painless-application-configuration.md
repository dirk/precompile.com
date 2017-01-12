---
layout:  post
title:   Fast and painless application configuration
date:    2017-01-12 22:00:00
summary: Improve engineer and end-user experience at the same time.
---

Rolling out features and controlling user experience requires being able to configure the application. Faster application configuration improves the safety of those roll-outs and feature changes.

Additionally, good roll-out practice involves a small initial deployment followed by gradual ramping up of traffic to the new code-path. If you can’t quickly change the amount of traffic exposed to the code-path, then you can’t quickly respond to detected errors in that new code.

#### A quick distinction

Keep in mind that in this context application configuration is separate from server configuration. Application configuration is used by the application code to control features; server configuration controls the foundation of the application server.

For example: an application configuration would be the ratio of requests using a given new feature, whereas a server configuration would be the URL of the database to use. The former can change frequently; a change to the latter should generally be accompanied by a server restart.

### Shape vs. value

One of the pitfalls of common configuration systems is that they combine both the shape (type) and value of the entries being configured. Environment-variable-based configuration is a classic example of this problem: eg. is the shape of a truthy boolean “true”, “yes”, “1” or something?

The shape should therefore be defined in the code, as the code that will use that configuration entry depends on the code that configures the shape of the entry.

The value should live in a place where it can be changed frequently; at [Everlane](https://www.everlane.com) we’ve used our primary database to good effect.

The result of this distinction is that a new configuration entry is introduced in the same version control change as the code that uses it. But its value can be changed on-the-fly by site administrators to let us easily ramp up new features and re-configure the behavior of existing features.

### Putting it into practice

At Everlane I built this system in three parts:

- A model layer (built up from a base `Config` model with descendants for each shape, eg. `FloatConfig`) which handles the serialization/deserialization of values and their persistence in the database.
* A service layer used by the application code to quickly access the current value of configuration entries.
* An administration user interface and API (for chat-ops!) that allows us to easily change values.

#### Making it low-overhead

One of the critical design decisions of the service is that it stores the entire current state of the configuration values in memory. (We don’t have a massive number of entries, so this is at most a few hundred bytes.) This means that reading the current value of a configuration entry only costs a Ruby hash look-up.

To ensure the values are up-to-date we use a [poorly-documented feature of the Puma web server](https://github.com/puma/puma/blob/f5f23aaac7aaccff1b6b138d93dd4b1755ebf1c2/lib/puma/server.rb#L571-L574) to reload the values *between* requests:

```rb
module ConfigStuff
  class PumaMiddleware
    RACK_AFTER_REPLY = ::Puma::Const::RACK_AFTER_REPLY

    def initialize(app)
      @app = app
    end

    def call(env)
      env[RACK_AFTER_REPLY] << lambda { self.reload_config }

      @app.call env
    end

    def reload_config
      # Make the configuration service reload its values.
    end
  end
end
```

### The end result

Having easy-to-control, low-overhead configuration made a noticeable impact on how we deployed and maintained features: removing the hassle of environment variables (and associated overhead of restarting the servers) made it easier for us to control the behavior and let us be more responsive to bugs.

The definition (and light type-checking) of the shape of values also simplified our application code: no longer did we need to do hacky conversion of environment variable strings into floats, booleans, etc.

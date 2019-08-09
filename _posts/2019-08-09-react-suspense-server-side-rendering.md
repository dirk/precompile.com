---
layout:  post
title:   React Suspense server-side rendering with synchronous host languages
date:    2019-08-09 1:10:00
summary: Using React Suspense and resources when pre-rendering from Ruby.
---

When the React team [introduced](https://reactjs.org/blog/2018/11/27/react-16-roadmap.html#react-16x-mid-2019-the-one-with-suspense-for-data-fetching) the concept of Suspense it was—and is—as a beta. One of the missing pieces was suspense-compatible server-side rendering (SSR): official support is expected in late 2019 in tandem with a new asynchronous server-side renderer.

In the meantime a couple of community members have developed asynchronous suspense-compatible render-to-string implementations: [`react-async-ssr`](https://github.com/overlookmotel/react-async-ssr) and [`react-ssr-prepass`](https://github.com/FormidableLabs/react-ssr-prepass). Both implementations assume that the suspenseful fetching happens _asynchronously_, and both take tree-walking approaches to rendering (the former by implementing a new renderer and the latter by implementing a visitor to walk the tree before rendering).

These are good solutions if you’re in an asynchronous environment when server-side rendering: for example running Node.js and pre-rendering within the Node.js environment. However if you’re pre-rendering from a host language such as Ruby or Python (normally via an embedded V8 library) then these solutions are unnecessarily complex and don’t pair well with the _synchronous_ execution of functions/methods/etc. from those languages.

The following lays out a simpler approach for doing suspense-compatible server-side rendering in a synchronous host language environment.

### Synchronous callbacks

We can start by exposing a data fetching function from the host language to the embedded language; in this example using Ruby and the [`mini_racer` gem](https://github.com/discourse/mini_racer).

```rb
find_widget_by_id = ->(id) do
  Widget.find_by_id(id)&.as_json
end

context = MiniRacer::Context.new
# Bundle is the output from the bundler, eg. Webpack, Sprockets, etc.
context.eval(bundle_source, filename: bundle_path)
context.attach('HostApi.find_widget_by_id', find_widget_by_id)
```

In our front-end when server-side rendering we’ll now be able to call `global.HostApi.find_widget_by_id`. V8’s execution will halt, control will be passed back to Ruby to execute the `find_widget_by_id` proc, and then control will be returned to V8 with the proc’s return value.

Our JavaScript code will therefore be executing _synchronously_ when we call that function.

### Environment-aware resources

When we implement our resource in the front-end we’ll now have two code-paths to take: a synchronous one if we’re server-side rendering, and an asynchronous one if we’re browser rendering.

```js
import { unstable_createResource } from "react-cache"
import { fetchWidgetById } from "..."

function createServerSideResource() {
  const cache = new Map()
  return {
    read: id => {
      if (cache.has(id)) {
        return cache.get(id)
      } else {
        const widget = global.HostApi.find_widget_by_id(id)
        if (!widget) {
          throw new NotFoundError(...)
        }
        cache.set(id, widget)
        return widget
      }
    },
  }
}

const WidgetResource = isServerSide()
  ? createServerSideResource()
  : unstable_createResource(fetchWidgetById)

export default WidgetResource
```

Now in a component we can use `WidgetResource.read` in both server-side and browser environments.

```jsx
import WidgetResource from "./WidgetResource"

export default function Widget({ id }) {
  const widget = WidgetResource.read(id)
  return <span>{widget.name}</span>
}
```

#### What about errors?

For error handling when server-side I generally let the error bubble up through the stack, catch and report it to the error monitoring service of choice, and then have the pre-rendering silently fail. That way for the user the page just takes slightly longer to load, which I believe is better than loading with error content briefly visible and then—if the browser render succeeds—flashing over to the non-error content.

This is okay because pre-rendering is an optimization. It's there to make the experience faster for users and provide better SEO, but its results are not authoritative. A single-page app with pre-rendering can still function without pre-rendering.

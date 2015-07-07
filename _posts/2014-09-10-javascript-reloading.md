---
layout: post
title:  JavaScript reloading
date:   2014-09-10 16:00:00
---

Yesterday I published an exciting little piece of JavaScript software I've been fooling with. It's called [dy.js](https://github.com/dirk/dy.js) and I hope it can set a model for the future of client-side (and possibly some day server-side) JavaScript development.

Our current model of client-side JavaScript programming is very much reminiscent of *cavemen* banging sticks together. Save the file, reload the page, do a couple actions to test if your code worked, check for an exception in the console, save again, reload again, etc. That's not very elegant, and it's definitely not very fun.

dy.js [brings dynamic code reloading](https://github.com/dirk/dy.js/tree/master/ext/reload#readme) to JavaScript in the browser. Your code is packaged up into *modules*, whose files are then reloaded dynamically whenever they're changed. Modules can provide loading and unloading handlers to deal with binding/unbinding DOM events and such. Every module is just a single JS object, so references to that module can remain constant throughout the lifetime of the page. Reloading a module is simply just (re-)calling the module function on that object to set new properties/methods/etc. on it. It's simple, elegant, and in most cases should *just work*.

This should—in my opinion—be seen as a starting point for rethinking how we approach JavaScript development. It's not the 1990s any more, so why put up with that tedious hold-over  "reload the page"?

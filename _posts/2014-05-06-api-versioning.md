---
layout: post
title:  API versioning
---

While working on stuff at Fetchnotes I've spent some time considering and discussing the problem of versioning APIs. There seem to be two schools of thought on this: include version as part of the URL or include version as part of the `Accept` header. For example, [Twitter][twitter] and [Basecamp][basecamp] version by URL while [GitHub][github] versions by header.

Originally I had gone with the `Accept` header system with headers like `application/vnd.acme.v1` because it seemed (then) that versioning was part of the request instead of the resource (URL). Most of the common wisdom—both among colleagues and the [greater Internet](http://stackoverflow.com/questions/389169/best-practices-for-api-versioning)—follows this reasoning.

But I have decided not to do this for my API work. I'm versioning in the URL. There are a few reasons for this:

* I want URLs to last. If we as developers can't avoid link rot in our APIs—which we build for other developers—how can we claim to build more complex URL-based services, let's call them *websites*, for regular users? A version is an integral part of your access pattern for a resource, therefore it should be part of the description of that resource, and the description of a resource is its URL (or URI if you're stuck in pedantry-of-formality land).
* I want URLs to be descriptive. This builds upon the previous point; a URL should describe everything relevant about a resource. This is part of our common language of REST APIs. Our interaction with these APIs are built upon two variables: the HTTP verb and the resource. Varying the version of the API we use may vary the properties of the resource we're interacting with, therefore the version should be part of the resource. For example, say we have a `widget` resource that has a `turn` action. In version 1 of our API that `turn` goes "left", but in version 2—for some complicated reason out of our control, thanks management—it goes "right". This distinction is critical to the resource; it's not something to be stuffed away in a header. In keeping with the [specification of HTTP][http], the `Accept` header should be specifying the **type** of response I want from the API, **not** the function performed to get that response. Therefore, the distinction between function should be part of the resource itself, ie. in the version segment of the URL.
* I am pragmatic and/or lazy. Some complicated regular expression/manual parsing of the Accept header is just asking for breakage down the road. I also think the code for versions should be separate. A controller should handle its resource(s) in just one frame of logical operation (one version). This practicality-via-separation point was raised by [Thomas](http://thomascannon.me/) and I wholeheartedly agree with him. Following this pattern with `Accept` headers would mean I would have to mess with the routing engine of the service to pay attention to the URL *and* the header when dispatching to controllers. I have yet to see a clean, elegant solution to this issue in either Rails or Node.js-land, and crafting such a solution is not a piece of code debt/responsibility I'm eager to take on. Therefore I'll let the routing engine take it easy and just check the version in the URL.

[twitter]:  https://dev.twitter.com/docs/api/1.1/get/statuses/home_timeline
[basecamp]: https://github.com/basecamp/bcx-api/
[github]:   https://developer.github.com/v3/media/#request-specific-version
[http]:     http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.1

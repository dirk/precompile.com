---
layout:  post
title:   Taking everlane.com global
date:    2016-11-22 17:15:00
summary: Going from fast in the US to fast around the world.
---

Since its launch in 2011, [everlane.com][] has been a US-focused site. Although there were some short international shipping tests and shipping to Canada, the site infrastructure—on both the front-end and back-end—was optimized for US customers.

However, the business is growing and part of that is expanding its availability to non-US customers. That means the end-user experience for those customers became a concern.

[everlane.com]: https://www.everlane.com/

### Understanding the work-load

You must know the problem before you can solve it. The critical issue for end users was the number of round trips their browser had to make from their device to Everlane's servers. From within the US it is rare to see round trips over a hundred milliseconds to AWS's eastern US datacenter. But from Europe it was on average a few hundred milliseconds and from Australia it could reach into many thousands of milliseconds (their consumer Internet is "clogged" when talking to the rest of the world).

If you only had to make one HTTPS request this wouldn't be that bad. But for an asset-heavy site like Everlane's a large number of requests (in the neighborhood of 25 or more) must be made to load the site. These requests cascade in a "waterfall" of requests over time, so the impact of round trip latency rose as the number of requests grew. (Parallelizing requests—which was done—alleviates this problem, but did not eliminate it.)

The solution to this problem is to put Everlane's servers closer to customers, so their round trip latency will be lower. This of course means developing the infrastructure into a primitive [distributed system][].

If everlane.com were a static site this wouldn't be too hard, but as an e-commerce site it needs to manage users, their shopping carts, placing orders, and a myriad of other tasks. The static assets on the site are distributed via a [CDN][], so they already load quickly. It's the dynamic functionality that required users' browsers to talk to Everlane's web servers.

[distributed system]: https://en.wikipedia.org/wiki/Distributed_computing
[CDN]: https://en.wikipedia.org/wiki/Content_delivery_network

### Data locality

Putting servers close to the user is only half of the solution. If the data those servers use is remote then they still must make expensive round trips to fetch that data. This is especially troublesome with query-happy ORM's like [ActiveRecord][]. Every single query incurs an expensive round trip, and rendering just a simple page would take almost 60 seconds.

Therefore the data needs to be next to the servers. Most databases provide read replicas: create a read-only replica of the database, have the writeable primary stream changes to the replica, and have your application read from the replica. This solves the round trip problem for reads, but not for writes. It also introduces a consistency problem. The data the application reads from the replica will be behind—in time—the primary.

[ActiveRecord]: https://github.com/rails/rails/tree/master/activerecord

#### Local and remote

The consistency and write problems are solved via [RPC][]. The application reads against the local replica. (Achieved using a custom library—named [active_replicas][]—to patch read-replica functionality into ActiveReocrd). However, it sends writes and other consistency-critical operations to a remote application instance located next to the primary database. That remote instance—next to the primary database—talks to the primary database and therefore sees a consistent view of the data. These RPC operations still incur a round trip cost, *but* it's only one round trip rather than multiple trips.

Due to the connection constraints of Everlane's platform a broker must be used to facilitate routing messages between the RPC client and server application instances. After evaluating various options RabbitMQ was selected as the broker because of its widespread use, reported reliability, and feature set. A custom library—named [fluffle][]—was also developed to run JSON-RPC over RabbitMQ.

[RPC]: https://en.wikipedia.org/wiki/RPC
[active_replicas]: https://github.com/dirk/active_replicas
[fluffle]: https://github.com/Everlane/fluffle

### Putting it all together

The application infrastructure changed—in terms of its data locality contexts—from homogenous to heterogenous. Originally it was just business logic interacting directly with an ORM connected to a single primary database. That evolved into a two-path setup:

1. For static data, such as from the content management system, the business logic still uses the ORM directly to read from the local read replica database.
2. When writes must be performed or the logic is sensitive to consistency the local business logic calls remote business logic over RPC. That remote instance then performs the operations on the primary database.

The final key to this significant change is doing it as a very gradual roll-out. The interfaces and implementations of these new features—RPC and read replica—are designed so that they could be easily turned off and on for each request. That results in edge case errors being caught with a limited impact on production traffic and damage to end user experience.

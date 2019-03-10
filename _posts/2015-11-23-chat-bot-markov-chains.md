---
layout:  post
title:   Markov chain generator in your chat bot
date:    2015-11-23 16:00:00
summary: Store Markov chains for each user in your chat bot; generate sentences from those chains for great fun.
---

Last night I coded up a basic [Lita](https://www.lita.io/) plugin for ingesting chat messages into Markov chains and generating sentences from those chains upon request; the source is available on [GitHub](https://github.com/dirk/lita-markov) and [RubyGems](https://rubygems.org/gems/lita-markov). It turned out this was almost trivial to accomplish.

### How trivial was it?

The Markov chain itself is a really simple concept. The [formal definition](https://en.wikipedia.org/wiki/Markov_chain#Formal_definition) concerns the probabilities of transitioning between various states, but the real-world implementation—in this case the wonderfully-named [`marky_markov`](https://github.com/zolrath/marky_markov) gem—is even simpler. It builds a hash mapping a key (a word or array of words) to an array of strings that can follow that key. The frequency/probability of different subsequent words is represented simply as the frequency of strings in that value array. (Note: these are all a arrays and not sets, so words/lists-of-words can and do repeat.)

#### An example

Let's say the bot has the following entry in its key-value store for the frequency of subsequent words when I say "hello":

```ruby
"hello" => ["world", "planet", "planet", "world"]
```

We can see that world and planet have a 50/50 chance of following hello. However, let's say I say "hello planet" on more time, the entry then becomes:

```ruby
"hello" => ["world", "planet", "planet", "world", "world"]
```

World now has a three-fifths chance of following "hello" when we ask the generator to create a sentence. It's not space-efficient by any means, but it is delightfully simple!

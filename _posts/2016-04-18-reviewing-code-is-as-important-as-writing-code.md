---
layout:  post
title:   Reviewing code is as important as writing code
date:    2016-04-18 19:00:00
summary: How to give your colleagues' work the attention it deserves.
---

Code review is a critical part of being part of a team of programmers. As humans we're all fallible and make mistakes when programming; tests, specs, and reviewing all play a part in catching those errors and oversights.

However, reviewing code isn't *just* about spotting bugs; it's also about taking yourself on the same mental journey that the author took. You're retracing their steps while also bringing your own perspective and experiences to bear on the feature/problem/etc. in question.

### It's dialogue all the way down

The programs we write convey function and structure to other people. The code is not just communicating literal intent to the computer; it's also communicating that to the humans reading that code in the future. (Comments of course aid in that communication.)

Code review is not that useful when you're merely looking for nitpicks, inconsistencies with the style guide, and so forth. But it *is* useful when you treat the code as a dialogue to which you can add your voice.

A literary editor doesn't seek to replace the author's work with their own. Instead they seek to provide subtle tweaks and helpful feedback so that the author can best convey their message. The process of review is much the same: making suggestions and highlighting issues for the author to consider so that they can produce their best work.

Keep in mind that the author spent real time and effort on producing the work under review. Is a cursory once-over and a few nitpicks really respecting what they did? They deserve your care and attention.

#### The two-pass method

Some important and humbling negative feedback I once received was that I cared too much: being too emotionally invested in the code-base resulted in comments that were too focused on the minutiae and not actually helpful. Learning to let go of small details that I didn't 100% agree with left me able to put effort towards focusing on the larger scope of the change and what it communicated.

I subsequently cribbed the following approach from a few more experienced colleagues:

1. Take a first read of the change-set, judiciously leaving comments only for egregious errors or style guide violations.
2. Pause to reflect on the change; go back and re-read the pull request description, relevant tasks in project management systems or reports in bug-tracking systems.
3. Do a second read to focus on the greater scope of the change and what the author is trying to communicate. Because you've already seen (and had your emotional reaction to) the small things you can put your attention towards the big picture.

This isn't a revolutionary method, but it has provided a simple routine that's helped me improve as a reviewer.

(My thanks to [Brad Fults](https://bradfults.com/), [Matt Newkirk](https://matthewnewkirk.com/), and others for their review and feedback on this.)

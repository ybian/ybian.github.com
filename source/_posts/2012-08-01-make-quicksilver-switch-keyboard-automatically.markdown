---
layout: post
title: "make quicksilver switch keyboard automatically"
date: 2012-08-01 16:29
comments: true
categories: 
---

If you are a Quicksilver fan and also a non-English native (like me), the following situation must sound
familiar to you: you activated Quicksilver, started to type a couple of letters and immediately 
realized that you forgot to switch to English keyboard first, then you switched to English keyboard and 
continued your workflow. Wouldn't your life be easier if Quicksilver can switch to English keyboard automatically
once it is activated?

<!-- more -->

Personally, I would love to have this feature so my workflow would not interrupted. Someone else share this same
opinion with me. There is a feature request on Quicksilver's issue tracking system on github.

What really triggered me to implement this feature rather than waiting for others to do that is a "Alfred vs Quicksilver" debate
in a local forum. An alfred user claimed that "my alfred can automatically switch my keyboard to English once activated. Can your
Quicksilver do that?". Oh..no, at least not yet. But I got the motivation to implement this, just for my favorite mac tool to
compete better against others.

With the help of nice guys at [Quicksilver](https://github.com/quicksilver), I sent out the pull request and made a lot of changes
per comments. It is still not merged yet, but I have the confidence that it will be. ;-)

The link to my pull request is [here](https://github.com/quicksilver/Quicksilver/pull/987). And the following is peak view of how
this feature works:

{% img /images/quicksilver-keyboard-switch.jpg %}

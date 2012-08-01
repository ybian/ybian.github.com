---
layout: post
title: "bring the best of two worlds - xvim"
date: 2012-08-01 17:32
comments: true
categories: 
---

I have been a happy Vim user for more than 10 years. I use Vim for almost all my text editing work, including 
system administration, command line editing, blog writing and coding, except for one thing: iOS/Mac programming,
for which I never feel comfortable to do in Vim. <!-- more -->Instead, I use Xcode. Xcode is the natural choice for iOS/Mac
programming as it has some neat features that other apps can hardly beat with, for example, the integration of
interface builder and app distribution, and more importantly, code completion (I cannot imagine coding objective-C
without code completion).

Xcode rocks in every aspect, except for... the editor. It lacks a lot of shortcuts that are in your Vim muscle memory.
e.g: `dd` to delete a line, `yy` to copy a line. Is it possible to replace the editor with a Vim simulator?

I did not think so until I came across [XVim](https://github.com/JugglerShu/XVim/) recently.

Per its `READEME`: 
> XVim is a Vim plugin for Xcode. The plugin intends to offer a compelling Vim experience without the need to give up any Xcode features.

It really works as advertised. With this plugin installed per instructions, I can finally turn the Xcode editor to behave just like Vim, and
all Xcode goodnesses are still there!

There is an issue, although. As the `timeoutlen` option is not implemented by that plugin, multiple keystroke mapping does
not work well in XVim, which is a problem to me, as I always have `;;` mapped to `ESC`.

I spent a couple of hours fixing this. My code changes have been sent out as a pull request [here](https://github.com/JugglerShu/XVim/pull/252).

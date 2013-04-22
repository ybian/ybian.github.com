---
layout: post
title: "A Quicksilver action to create new reminders"
date: 2012-08-15 15:27
comments: true
categories:
---

I recently switched to Reminder.app as my ToDo management tool, after upgrading to Mountain Lion. The only thing I missed is a hotkey window
to create a new reminder instantly. As always, I turn to Quicksilver for this kind of things.

So, I wrote a Quicksilver action (in Ruby). The following is the screenshot of how it works.
{% img /images/make_new_reminder.jpg %}

<!-- more -->

And here is the source code. Just drop it under `~/Library/Application Support/Quicksilver/Actions` and make it executable by using `chmod 755 <script>` in terminal (thanks to Luke for pointing it out), then restart Quicksilver.

{% include_code lang:ruby NewReminder.rb %}

Some notes about this script:

* The script is written in Ruby, and it uses 2 gems: `chronic` for parsing human readable date string and `terminal-notifier` for send notification using Notification Center. As `chronic` cannot work well with Ruby 1.9.3 as of this writing,
  I forced the script to be executed using system ruby (/usr/bin/ruby) which is of version 1.8.7 in OS X 10.8. Anyway, you need to install those 2 gems using `gem install chronic` and `gem install terminal-notifier`.

* As it uses Notification Center, it should only work on OS X 10.8+. But it is easy to be tweaked to drop notification or use other notification services like Growl.

* Some AppleScript code is embedded and executed by `osascript` to interact with Reminder.app.
  This script could be written entirely in AppleScript, but I chose Ruby + AppleScript way as
  it is much easier to write most part of the logic in Ruby.

* The usage of this action: assuming it is correctly installed, launch QS, hit "." to enter text mode, write your reminder message, hit tab key to focus on the 2nd pane, and choose "NewReminder" action. Your reminder should be created.

* You can set list and/or due date in your message. The list name should be prefixed with `@`. The due date string can be any human readable format understandable by [chronic](https://github.com/mojombo/chronic). Due date must be prefixed with `^` and be the last part of the message.
  For example, `call my mom @life ^tomorrow 6pm` will create a reminder `call my mom` in the `life` list and set due date to 6pm of tomorrow.

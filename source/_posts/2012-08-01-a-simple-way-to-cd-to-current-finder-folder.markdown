---
layout: post
title: "A simple way to 'cd' to current finder folder"
date: 2012-08-01 11:33
comments: true
categories: 
---

On my mac, I need to spend a lot of time working with Finder and iTerm (or Terminal), and I find 
it tedious to switch between those 2 apps as I often need to open the working directory of iTerm inside Finder, and the opposite, cd to the current Finder folder in terminal.

<!-- more -->

The former is easy to accomplish by using the magic `open .` command in terminal and I have been using it for years. Very handy! 
For the latter, I have tried several solutions but was never satisfied. Here are some solutions I tried before:

* *Pathfinder*: Yes, pathfinder is a nice replacement to Finder and has terminal integration. It has a lot of features, but what I really need is a little piece. So this is an overkill to me.
* *Visor (TotalTerminal) / iTerm Hotkey Window*: Both can be used to open an overlay terminal window, but not be able to `cd` to the current folder automatically. And, it was not flexible to be enforced to work
  in that overlay window. I want to work in whatever terminal session that is already opened in my iTerm!
* *Go2Shell*: This is a little program which allows you to open the current finder folder in terminal. It works but it is still not flexible either. It always opens a new terminal window, and cannot let you work
  in your existing session.

So I decided to implement my own solution. It turns out to be easier that what I expected. What I need is to write is a little AppleScript and create a shell alias.

First, I need to write a little AppleScript code as the following:

```
try
    tell application "Finder"
        set this_folder to (folder of the front window) as alias
    end tell
on error
    set the this_folder to path to desktop folder as alias
end try

set full_path to POSIX path of this_folder
```
Write the above code in a text editor and save it. I saved it as `~/bin/extra/GetCurrentFinderFolder.scpt`.

The script is self-explanatory, if you execute it in command line via `osascript /path/to/your_script`, it would output the path of the opened folder in Finder. 

Next, create a shell alias and add it in your `.bashrc` or `.zshrc`:

```
alias cdf="cd \"\`osascript ~/bin/extra/GetCurrentFinderFolder.scpt\`\""
```

Note that you have to carefully escape the quote marks like above.

Now, you can use the handy `cdf` alias wherever you want in your terminal! I have been using this for a month and really enjoy it.

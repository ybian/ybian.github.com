---
layout: post
title: "make spritesheet generation automatic using makefile and texturepacker"
date: 2012-08-07 11:23
comments: true
categories: 
---

In the current game that I am developing, I need to update spritesheets frequently to incorporate changes from my artist. As I was using zwoptex as the SpriteSheet generation tool,
the process that I followed was:

Open the zss file -> Select all -> Delete -> Import -> Select all -> Publish (assuming I had configured the publish location correctly)

It usually only takes one minute or two, but it becomes very tedious if you repeat it more than 3 times. So I decided to find a way to automate the process, and I found it.
<!-- more -->

The key to the solution is TexturePacker - a better SpirteSheet generation tool that supports command line. TexturePacker has a lot of amazing features, but the killing feature for me is
command line support, which makes automation easy to implement. Yes, you can easily write a shell script to automate your process of regenerating your spritesheets. But you can make the process
more seamless by integrating it into a Makefile that can automatically be called when you build your game in Xcode.

Read below for detail steps to accomplish this.

### Create a new target

* Select your project root node in Xcode left navigator (1), and click "Add target" (2), in the pop-up dialog, choose `OS X -> Other -> External build system`, name it whatever you want.

* Select the newly created target (3), leave build tool as `/usr/bin/make` and arguments unchanged, configure 'Directory' to wherever you plan to put your Makefile (4). In my case, I set it to "Resources" folder under my project.

{% img /images/create-target-in-xcode.jpg %}

### Add target dependencies to your existing target
* Select your existing target, switch to "Build Phases" tab, expand the first section titled "Target Dependencies", click "+" icon, choose the target you created in the previous step.

### Add the Makefile
* Create a new file named `Makefile`, add it to the directory you configured in the step "Create a new target". And, edit the file content to something similar to below:

{% codeblock An example of Makefile lang:sh %}
_IPADHD	= ipadhd
_IPAD 	= ipad
_HD	= iphonehd
_NORMAL	= iphone
TP	= /usr/local/bin/texturepacker
CV 	= /opt/local/bin/convert
TP_OPTS	= --data $(basename $@).plist --sheet $@ --format cocos2d --algorithm Basic --no-trim
SPRITESHEET_DIR = ../../assets/SpriteSheet

all:SpriteSheet.png

SpriteSheet-ipadhd.png:$(SPRITESHEET_DIR)/$(_IPADHD)/*.png $(SPRITESHEET_DIR)/$(_IPADHD)
	$(TP) $(TP_OPTS) $(SPRITESHEET_DIR)/$(_IPADHD)/*.png

SpriteSheet-ipad.png:SpriteSheet-ipadhd.png
	-rm $(SPRITESHEET_DIR)/$(_IPAD)/*
	for x in $$(ls $(SPRITESHEET_DIR)/$(_IPADHD)/*.png); do \
	    $(CV) $$x -resize 50% $(SPRITESHEET_DIR)/$(_IPAD)/$${x##*/}; \
	done
	$(TP) $(TP_OPTS) $(SPRITESHEET_DIR)/$(_IPAD)/*.png

SpriteSheet-hd.png:SpriteSheet-ipad.png
	-rm $(SPRITESHEET_DIR)/$(_HD)/*
	for x in $$(ls $(SPRITESHEET_DIR)/$(_IPAD)/*.png); do \
	    $(CV) $$x -resize 85% $(SPRITESHEET_DIR)/$(_HD)/$${x##*/}; \
	done
	$(TP) $(TP_OPTS) $(SPRITESHEET_DIR)/$(_HD)/*.png

SpriteSheet.png:SpriteSheet-hd.png
	-rm $(SPRITESHEET_DIR)/$(_NORMAL)/*
	for x in $$(ls $(SPRITESHEET_DIR)/$(_HD)/*.png); do \
	    $(CV) $$x -resize 50% $(SPRITESHEET_DIR)/$(_NORMAL)/$${x##*/}; \
	done
	$(TP) $(TP_OPTS) $(SPRITESHEET_DIR)/$(_NORMAL)/*.png
{% endcodeblock %}

This Makefile set a "source directory" as the prerequisite of the target `SpriteSheet-ipadhd.png`, whenever a change is made to the directory, this target will be updated, using the TexturePacker
command in that rule. Besides, there are 3 other rules for -ipad, -hd and normal spritesheets as well, whenever the `-ipadhd` version gets updated, other versions of SpriteSheets need to be updated
as well. In those rules, `convert` command is used to resize images before passing through them to TexturePacker.

### Done
With this setup, it now becomes a breeze to update your spritesheet: just throw your new images into your assets directory and then rebuild your game. Everything will be done automatically for you!

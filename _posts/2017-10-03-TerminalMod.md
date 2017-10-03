---
layout: post
title:  "Make my Terminal pretty"
date:   2017-10-03
categories: posts
author: devonorourke
tags: 'computery'
---

# Why modify? Because it's cool, that's why.
- Do you use a Mac? If yes, continue. If no, ... well [you](https://media.giphy.com/media/ARj2OMThsPoAw/giphy.gif) get nothing, you lose, good day sir.  
- Do you use Terminal? If yes, continue. If no, [try here](http://mac.appstorm.net/how-to/utilities-how-to/how-to-use-terminal-the-basics/).  
- Do you hate the Terminal default parameters and [the available alternatives](http://osxdaily.com/2011/05/02/change-the-appearance-of-terminal-windows-quickly/)? If yes, continue!  

So what's this about? Really it's about me not liking the color palette of my Terminal and wanting to change two basic features:  
1. Get access to a much larger set of potential color palettes to use within Terminal.  
2. Specify the colors in the output of a few commands like `grep` and especially `ls`. It turns out this is way easier with Linux than with Mac, which, well, of course.  

Let's deal with each of these separately...  

## Get a big palette
There are a lot of sites out there that will help you generate a suite of custom colors. I wanted to be as simple as possible and not design any of them myself, rather, just look through a list of these custom palettes and pick which one (or ten) I wanted. [This color editor](https://github.com/lysyi3m/osx-terminal-themes) does exactly that, and because it's through a Git repository it's very straightforward to implement. Install directions are right on the README.md page but I didn't follow them exactly. Here's what I did instead:  

**Step 1** - scroll through the main page and find a palette you like. Remember the name. I liked [this one](https://github.com/lysyi3m/osx-terminal-themes#birdsofparadise) because apparently I subconsiously wish I went to Tufts given that color scheme...  

**Step 2** - Open up your Terminal program and clone the repo as follows:

> *Side note: I have a generic `gitstuff` subdirectory within `My Documents` to pace cloned repos for little projects just like these... that's used in the example below*  

```
cd $HOME/Documents/GitStuff/
git clone https://github.com/lysyi3m/osx-terminal-themes.git
```

**Step 3** - Continue using terminal to find the path to the file you want specifically. Remember the name from **Step 1**?  
```
cd $HOME/Documents/GitStuff/osx-terminal-themes/schemes
pwd
```

**Step 4** - Click on the "Terminal" icon and open up `Preferences`. There are four main icons across the top of the Preferences window - click on the one labeled `Profiles`. From that Profiles window, click on the gear-shaped icon at the bottom (often called a `settings` icon in a lot of software), then click "*import*". From that point you just navigate to whatever directory your output of the `pwd` command generated from **Step 3** and select the file name you wanted from **Step 1**. This should generate a new icon in that *Profiles* window and you can click on the `Default` icon right next to the gear you clicked on previously to make this new color scheme the one that you get every time you open the Terminal program (otherwise you just need to go into *Preferences* and select it each time).  

This gives you a really broad palette of color shemes, but you are of course limited by whatever is among these presets. If you look through these files themselves, they're in a funky .xml code - here's a snippet:
```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
	<key>ANSIBlackColor</key>
	<data>
	YnBsaXN0MDDUAQIDBAUGFRZYJHZlcnNpb25YJG9iamVjdHNZJGFyY2hpdmVyVCR0b3AS
	AAGGoKMHCA9VJG51bGzTCQoLDA0OVU5TUkdCXE5TQ29sb3JTcGFjZVYkY2xhc3NPECcw
	LjEzMzMzMzMzMzMgMC4xNzY0NzA1ODgyIDAuMjQ3MDU4ODIzNQAQAoAC0hAREhNaJGNs
	YXNzbmFtZVgkY2xhc3Nlc1dOU0NvbG9yohIUWE5TT2JqZWN0XxAPTlNLZXllZEFyY2hp
  ```
  
If you understand (a) XML, and (b) want to sift through the [list of ANSI color codes](https://gist.github.com/chrisopedia/8754917), well good on you, it's all right there for the mixing and (endlessly) matching. If you're like me, you're hoping someone has programatically come up with a GUI that just lets you select all this by color, and generates the script that these custom palettes are based on. And of course it has...  

Try [this site](https://terminal.sexy/). My brief advise: look for the `Scheme Browser` window in the bottom/middle, and play with those selections first. If you find one you like, click `Export` only after you've selected your terminal type, which turns out to be `Terminal.app`. You can download the text file or just copy and paste the output generated into a new file, and provided you know what the path to that file is, you can follow the same instructions above to use that uber custom design.  

But default, whatever palette you choose - custom or not - won't necessarily retrieve colored outputs of your `grep` and `ls` commands. We'll change that next.

## Give me color!  
What's the motivation here? I like color and I it bothers me when I can't automatically differentiate between directories and files. It turns out there is a sort of generic response to this problem, which essentially amounts to a "turn coloring on" command - see [this site](https://softwaregravy.wordpress.com/2010/10/16/ls-colors-for-mac/) for a blog post about doing just that. If that's really all you need, you can also customize that a bit further with [this handy color generator page](https://geoff.greer.fm/lscolors/).  

Getting any color in and of itself is a win, but I deal with a lot of different file types and it's helpful to easily distinguish certain file types from another (ex. `file1.txt` vs. `file2.csv`) - see [this thread](https://superuser.com/questions/468966/colouring-output-of-ls-according-to-file-extension) for another example and discussion. What I really want was a way to customize *any* file type as well as directories. Coloring certain file formats is a quick way around this.  

Truthfully, all we're really going to do is something that is a specific example of a more generic process - mucking with the `.bash_profile` (or alternatively `.bash.rc`) file to generate a few *alias* commands so we don't have to type and retype our custom colored outputs from `grep` and `ls` commands. Unfortunately, it's not as easy with a Mac as it would be with a Linux machine, so we're going to have to do a little bit of a workaround to get it all together.   
- don't know what an `alias` is? that's okay - [try this](http://www.linfo.org/alias.html)
- don't know what a `.bash_profile` is? I didn't either! [try this](http://www.joshstaiger.org/archives/2005/07/bash_profile_vs.html)... turns out the answer is different depending on Linux or Mac OS  

> Note that you *do not* need to have a custom color palette selected within your Terminal app to generate a custom color output generated from things like `ls` and `grep` commands. While these will work with the steps described above, the first half of this post isn't required.  

**Step 1** find your `.bash_profile` file:
Unless you've made a lot of changes already, by default your Terminal app should open to something like `/Users/{yourname}/`; in my case when I open up a new Terminal app it looks like `/Users/do`. From there you should automatically have `.bash_profile` that is accessible by throwing a "do not ignore hidden files" argument as follows:  
```
## where are you?
pwd

## if not home, go there:
cd $HOME

## bring up .bash_profile
ls -a
```
This should generate a list of files in your `Users/{yourname}` directory including the `.bash_profile`. You can now edit that profile by appending the following info using your favorite text editor (ex. nano, vim):  
```


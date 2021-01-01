---
layout: post
published: false
title: Learning Linux - the wrong way
date: '2019-11-07'
subtitle: how not to do it
tags:
  - Linux
  - Ubuntu
---


I've dabbled with Linux a few times over the years - mainly on expired laptops that have given up the ghost (Win XP), but never stuck at it. Simply, it was too much hassle to try to get to grips with , in the limited time outside work (hah - this was before kids came along).
Quite often on  Twitter, I see barbs aimed at Windows users, but, I don't know anyone who has a Mac, and have never worked anywhere that wasn't a 'Microsoft shop'. But Mac owners also seem to complain.

As I've become slightly more adept at R, and become more exposed to how other people work, it is clear that there are a bunch of Linux R-users who seem to get on just fine. Furthermore, I'm aware that there are a bunch of [command line tools that can make data science a lot easier](https://www.datascienceatthecommandline.com/). 

I've had a copy of that book for a while but never done anything with it. 
Then my fellow NHS colleague Chris Beeley tweeted out that we (NHS-R types) should all learn Linux, and I think, "Yeah, maybe I should give that another go". 
Who knows, maybe in some post-apocalyptic future, we won't have flashy computers and we'll have to make do with things that run on 256MB of RAM and then we'll all be wishing we had a copy of Puppy Linux on a non re-writeable CD.
Slightly more beneficially, if I write a package, I can test it on Windows and a Linux build. Yay!

## Day 1 - bye bye Windows. 


I liberated a laptop I'd given to my eldest son that was not being used (I'd hoped he would show some interest in coding but he is clearly not a self-starter).  I found a copy of an old Ubuntu distro (did you see how I casually threw that in there, like a pro) lying around, and,  with barely a care in the world,  I over-wrote Windows and installed Linux on it. 

"Distro" is a term for distribution - which is how Linux users refer to the different types/ versions of Linux that are on offer. 
I have to say I find this really confusing. For years, I've wanted to install Linux Mint, but there are 3 different desktops for that, and I don't know why that is, or which is best, or how anyone knows that.  Do you have to install it 3 times? And then try and remember which you liked best? Who has time for this?

Ubuntu, as far as I can tell, is pretty much the 'beige' of the Linux world. Well, that'll do me. 
Unfortunately, the distro I had, was what I'll call an 'inbetweener'. Ubuntu releases stable versions that come out every few years, and which each have guaranteed 5 years support.  These versions all end in LTS (Long Term Support).

In the distant past I had the 2014 era LTS, but could no longer find it, so installed a 2015 version that came on the cover of a magazine. My plan was that Ubuntu would update itself to the next LTS version, as, if there is one thing I've learned, Linux is very good at keeping itself up to date. 

Sure enough, once installed, it began updating, and I was prompted to update to the 2016 LTS version. 

If you are new to Linux, one of the first things you need to get used to is using the terminal, where you issue commands.
You also need to get used to being prompted to enter an adminstrator (sudo) password every time you make changes.


So I entered my password, and sat back to watch the update progress. 
Only it couldn't do it. The updates kept failing. My 'apt' was totally banjoed. Or words to that effect. 
I'm not even sure what 'apt' is, but without it, you are in 'No UpdatesVille'

Eventually, I searched the error message, and eventually found a resolution, which was not the accepted solution, but one much further down the page. 

This involved a lot of faffing around typing very long, complicated commands. 
"Why can't I copy and paste into the terminal?" I moaned, as Ctrl-C and Ctrl-V failed miserably.

Yes, yes, I know NOW.

You need the shift key. So, copying from the webpage with Ctrl-C was fine, but I needed Ctrl-SHIFT-V to paste into the terminal.


I should have quit there, dear reader. But no, I got greedy. 
I was offered to update to the 2016 LTS version. Heck yeah!

I was of course warned that this would take some time. I went for it.  Then I got bored and tired ( it was past 1 am at this point), so I thought I'd shut the laptop screen over juuust enough to stop our new crazy kitten standing on the keyboard.
Big mistake.

I went too far. 

The laptop went to sleep, then wouldn't respond. 
Nor would it boot. 
I'd completely borked it. 
At 1.40 am, I went to bed, and spent a restless night where every time I stirred, I remembered I'd totalled my laptop.

What a waste of time.


Actually, no, no it wasn't.

Lessons learned: 
- Using Ubuntu? Install a LTS version
- Ctrl-C and Ctrl-V won't work in the terminal. Ctrl-SHIFT-C and Ctrl_SHIFT-V are what you need
- Don't power off during an upgrade. Step away from the laptop. Enforce a no-go zone. Do what you have to. But don't let it power off.

TBC....







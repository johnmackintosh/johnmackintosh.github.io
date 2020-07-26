---
layout: post
published: true
title: Learning Linux - the wrong way - day 2
date: '2019-11-09'
subtitle: Unborking the borked  laptop
tags:
  - Linux
  - Ubuntu
  - rstats
---

## Recap

I'm trying to learn some Linux. Ostensibly to do some data science at the command line, because it feels like something I might need to know at some point. I have reclaimed an old Windows laptop ( poorly specced, with missing keys and a penchant for sending the cursor up several lines at once with no prior warning), and I installed a Ubuntu distribution on it. 

I then decided to update the distribution, and accidentally lost power during that install. So now, I have a laptop with no Windows, no Linux, and it won't boot


## Day 2

Today the clocks go back 1  hour. That's supposed to mean an extra hour in bed. That doesn't happen when you have kids though, so, having staggered to bed at a stupid time in the morning, I am being woken a few hours later.  Miraculously, there is no fighting or squabbling (amongst the kids that is), so once breakfast is out the way,  I pick up the laptop and retrace my steps from the previous night:  

- install 2015 version of Ubuntu from cover disk 
- fix the broken APT, which I later discover means Advanced Packaging Tool. Without it, I can't update. 
- Peform a proper installation of Ubuntu 2016 LTS. 

At last, I have a decent installation. I take a look at the software centre, which is where you find new programs and tools, and see some new options available.  A python interpreter is already installed,  Julia is available, and I eventually find an older version of R, so of course, I install that. 

It really is old though, so I find myself browsing to CRAN and downloading the latest version. Thankfully,  I don't need to do any command line kung fu to get it installed, which is kind of cheating, but I just want it on there.

Having fired it up, and being glad it worked,  I decide to go for RStudio as well. 

This is where things start to get interesting - it doesn't install because of missing / out of date files. 

So it's back to firing up the terminal, and `apt-get install` to try and find the stuff I need.  Ubuntu is pretty good at telling you what is going on, and what the problems are, so I didn't feel too lost. There was a bit of googling going on,  but nothing out of the ordinary. 


I've written an R package, which builds fine in Windows. I tried a random Linux build using RHub's online tool, and it failed. So, I wondered if I could download my package from github and build it in Ubuntu. 

Once I'd got RStudio installed,  I begin installing packages. I figured the biggest bang for my buck was installing tidyverse. 
Blimey, that took ages. A few things failed (dependencies of dependencies), so there were some false starts getting everything needed, and then it began to build the package. 

I had no idea it would take so long. Seriously, it feels like Windows has the edge here. 

[My first plot in Ubuntu](https://twitter.com/_johnmackintosh/status/1188450025656987650/photo/1)

It's worth reading that thread as Chris Beeley, Paul Drake and others all chipped in with really good advice.


Various other packages were installed. Data.table was a breeze. Eventually I had what I needed ( data.table, zoo, ggplot2 and magrittr) so I downloaded my package files.

I opened them up in RStudio, and verified that it passed all checks, then built it. 


![runcharter]({{site.baseurl}}/img/2019-11-09-runcharter-ubuntu.png)


Success, package built, and working in Linux. 

I tweeted out some pics, and spawned a bit of discussion amongst several other NHS colleagues  - you know who you are.
Later on that night, I tried to burn Linux Mint to disk. I used the built in 'brasero' disk burner, but it failed. Possibly due to a combination of poor quality disk, and a rickety CD/DVD drive. 


Apparently, I should be using a usb-key for this, but it's been years since I had need for a usb-drive, so did not have one to hand. 

I finished off by browsing through an ebook I picked up for free on Amazon (Linux for Beginners by Jason Cannon happened to be on offer a day or so earlier) 

It starts off by explaining file directories, and how to navigate them, which is definitely going to take a bit of practice, for someone so used to 'point and click' navigation. 


So, to recap, some slight progress: 

- 2016 LTS installed
- R , RStudio and VS Code installed
- Checked and built my R package on Ubuntu - woohoo
- Informed that Mint was the way to go, as was usb installation
- Lots of practice with apt-get upgrade, apt-get install and apt autoremove



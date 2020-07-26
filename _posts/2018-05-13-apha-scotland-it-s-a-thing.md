---
layout: post
published: true
title: AphA Scotland - it's a thing
date: '2018-05-13'
author: HighlandDataSci
tags:
  - ggplot2
  - dplyr
  - rstats
subtitle: Reflections on AphA Scotland launch event
---
On Tues 8th May there was only one Scottish based member of the [Association of Professional Healthcare Analysts](https://www.aphanalysts.org) (me) but on Wed 9th May that number rose to around 80
 with the launch of the the Scotland AphA Branch. 
 
 The event took place in the very nice Perth Concert Hall, and consisted of several great speakers plus a series of workshops. I particularly enjoyed hearing from Mohammed Mohammed, who talked about setting up the NHS-R community, and how we might overcome the resistance to R in some quarters within the NHS.  
 
In later discussions after the event, there was some conjecture that this might be because traditional IT depts cannot provide support for R. Thinking back to when I first got it installed, I think I was told that I would get no support for it, which I was totally cool with because I just wanted to ggplot EVERYTHING.  (Actually, I based, and latticed, before I ggplotted, but you get the point).

The beauty of R though is that "support" is plentiful and 24/7 in the shape of the #rstats community.  I think if someone has got to the point where they want to use R, they are well into "power-user" territory and beyond the scope of regular IT support anyway. Not only that, but to get to that point, they have almost certainly mastered the black arts of Google-Fu and Stack Overflowing like a demon. Therefore they are the sort of self starters who are not going to be bothering IT in the first place. In other words, if we want to use R, let us use R (responsibly). 

By virtue of being member number 1, there was a suggestion early on in the planning stage that I be involved on the actual day. Initially it was suggested that I might have to get up on stage with the Actual Proper Really Important People. Thankfully, this idea got canned and evolved into being able to co-host one of the workshops (I never want go up on a stage, unless it's behind a drum-kit), with the very talented [Mr Neil Pettinger](http://www.kurtosis.co.uk/index.htm).
 
 This gave us a chance to demonstrate some of the patient flow graphics we've been iterating on - Neil had originally drafted Excel versions and then I tied my hand at replicating them in R. [I blogged about this towards the end of last year so go take a look there for some more background](https://www.johnmackintosh.com/2017-12-21-flow/). 
 
Our aim was to make the workshop a conversational, interactive affair, and I think we managed it. 
As Neil is based in Edinburgh, and I'm up in Inverness, most of our communication has been via email or Twitter DMs. We had maybe 2 phonecalls prior to the event. On the day before, we tried doing a rehearsal via internet video conference but my 4 year old twins managed to gatecrash that and it was a bit of a disaster. 

There was a fair bit of trepidation on my part before the first session, but it went well - people asked questions, which is always a good thing. In all we had to run the workshop 4 times, which meant we missed out on the other sessions running in parallel.

I would also have liked to have been able to participate in the discussion about AphA Scotland moves forward. 
My one hope is that future events remain in Perth, which is a lot more 'central' than the usual Edinburgh/ Glasgow locations, or, another way to look at it, is "equitably inconvenient" for everyone. 

The slides that I put together for our workshop are hosted here:

[DataTransitions: visualising and animating patient flow](https://cdn.rawgit.com/johnmackintosh/AphA/9a9df367/AphA.html#/ "DataTransitions: visualising and animating patient flow")

This shows the original and revised Excel plots, plus the dplyr and ggplot2 code in a step-by-step guide to creating the R equivalent. 

I'm still undecided on making presentations in R. For our purposes, PowerPoint might have been absolutely fine, BUT, for those who were new to R ( I'd say it was a 50/50 split in terms of our workshop attendees between those who'd seen/used it, and those who hadn't), it was quite nice to demonstrate nice graphics, and also say, "Yes, these slides you're looking at, these were put together using R".  I had also planned to spin up a Leaflet map centred on the concert hall with a big "You are here" sign but didn't get round to it.

One other cool moment, at least as far as this blog is concerned, was speaking to someone, who upon realising I was from the Highlands, put 2 and 2 together and asked "are you... HighlandR?"
Yay! Someone reads this stuff!

Big thanks to Paul Stroner (Apha CEO), Val Perigo ( Administrator extraordinaire), Scott Heald, Peter Knight and Neil Pettinger for organising the event, and to all those who attended. You wonderful people. 

I'm looking forward to seeing what happens next, roll on the next event and hopefully, some training too :)

---
layout: post
published: true
title: July(ish) Update
date: '2017-07-27'
subtitle: A belated review of things I 've been up to but not blogged about until now
---


So, merely days (ok, weeks) [after I suggest that I never have problems coming up with snazzy blog post titles,](https://twitter.com/HighlandDataSci/status/878360947852627970) here I am with what has to be the most uninspired post title ever.

And yet, it will have to do, as this is a sneaky way of getting a post in before the month of July passes me by. 

In terms of R this month I have been mostly :   

- getting acquainted with data.table     
- going bang bang (well once at least) with tidy evaluation in dplyr 7.0  

- working on an R package to help me create quite a lot of certain types of charts very quickly.  

Basically this should just be taking a group of functions that work well in interactive dplyr, and creating one super function to run them in order. 
Somewhere along the line it's become more complex, but I will get there in the end. 

In the last few weeks or so I've had a look at seplyr - which aims to reduce the complexity of tidy evaluation. 
Of course, this is very much opinion based -  I've seen  a couple of posts recently questioning the need for the package. 
My opinion, as a lone R user, is that this package will be beneficial to those who :  
- Don't / can't use R exclusively     
- Can't get to any of the awesome R / data science/ data viz conferences that seem to be an almost monthly occurrence in the US.  

Unless the dplyr gurus have any plans to rock up to [Inverness](http://www.explore-inverness.com/) any time soon (please do, the weather's hit and miss but we're a friendly bunch), I am reliant on R wizards on the internet, or my own brain power, to work out what I need to do. There are a few more posts starting to seep through, so hopefully will be able to suss out some of the more complicated stuff. 

It's quite hard to pick this stuff up when constantly switching between tools. As an example, I had to update a report on Monday morning. 
This involved: 

- updating some VBA to move Excel attachments to network drive via Outlook  
- running an R script to read .xlsm files and merge these to one .xlsx file (easier to use R than trying to troubleshoot SSIS and it's inability to parse certain types of Excel files).  
- running SSIS jobs and SQL queries to merge and update the data  
- more R code to analyse & visualise as static plot, before finally  :
- updating a QlikView application and distributing reports. 

Then repeat for another data set.

Now on a good day, with a cup of coffee and a headwind, I can get this done in 10 mins or so, but it's taken a while to get this slick and a lot of that was down to switching costs of tackling each problem with the relevant tool.  Anything that makes switching between different tools easier / reduces cognitive burden is  good thing, even if it only turns out to be a temporary crutch to free up time to master the deeper theories. So, I approve of seplyr. At the end of the day, its getting stuff done that counts - right?

### data.table
Wow this is fast! It also dramatically reduces the amount of code required. I've shied away from it in the past because it looked a bit incomprehensible IMHO, but I know now that if you can immerse yourself in it for a few days it does all come together.  As someone who uses SQL, I  annoyingly still didn't _quite_ get the whole " i,j, by" implementation, at least not initially, but it does make sense as you get further into it.  I was testing on 2.4M rows of data and it did what I wanted in 0.4 secs (using the microbenchmark package). Dplyr was  typically clocking in at a not too shabby 2.5 secs, and actual SQL was so slow I couldn't justify running the code more than once as a comparison. 

I am struggling with one operation, where i just want " i, by" and no 'j' - have seen a couple of posts on the [font of all knowledge and wizardry](https://stackoverflow.com/) so just need to get some clear head space and and get to work.  As a last word on data.table, I found [this little beauty](http://stephlocke.info/Rtraining/tablewrangling.html) courtesy of [Steph Locke](https://itsalocke.com/) to be super helpful in getting me started. If you are thinking about using data.table or just  'DT curious', then you should take a look at that guide.


### Away from R, I've been:
- using QlikView (run charts and Statistical Process Control, plus some work for some colleagues, incorporating boxplots, histograms and other stuff that ggplot2 users take for granted but which turn out to be a lot more complex in other software). Seriously - we can't credit Hadley et al enough for ggplot2. Try and make a boxplot in pre 2016 Excel. Or a histogram in QlikView (not quite so painful, but still not massively straightforward). It's a piece of cake in ggplot2, and so much of the work is done for us. 


- I built my first Qlik Sense app - just for fun, looking at whether Metallica have slowed down recently. (Spoiler alert - no, they haven't, but it is seriously hard to find reliable BPM info on the internet).

- I also managed to break my laptop ( the dangers of dual booting). On the plus side, I am now rocking a brand new SSD powered device and ignoring the colossal expense (speculating to accumulate, I hope), it is super fast and relatively future proof (if my wife sees the word 'relatively' she'll be slightly unhappy). So I have no excuse not to post more often.

Sorry about that.

Until next time..

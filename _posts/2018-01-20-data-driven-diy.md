---
layout: post
published: true
title: Data Driven DIY
tags:
  - rstats
date: '2018-01-20'
subtitle: Hardwired...to Shelf Construct
---
## Which fixing should I buy?

I have a bathroom cabinet to put up. 

It needs to go onto a tiled plasterboard (drywall) wall. 
Because of the tiles, I can't use the fixings I normally use to keep heavy objects fixed to the wall. 
And bog standard rawlplugs aren't going to do the job.

So what should I buy?

YouTube to the rescue - more specifically, this fine chap at [Ultimate Handyman](https://www.youtube.com/user/ultimatehandyman/ "Ultimate Handyman").

Not only does he demonstrate [how to use the fixings](https://www.youtube.com/watch?v=mIEY3E4fiDk&list=PLLV8cNgalvMnX42QROPRfVH8UrSB0cqxp), but also produced this strangely mesmerising [strength test](https://www.youtube.com/watch?v=ECDysfNhx_E) showing how much weight the fixings support before the plasterboard gives out. 

As well as the strength of the fixing, I need to consider the price of the fixings, and also, the size of the hole required (which in turn, will also impact the overall cost of the job if I have to buy new drill bits).
Plus - I've never had to drill into tiles before so the smaller the hole, the better.

Here's my code to try and visualise what to buy:

<script src="https://gist.github.com/johnmackintosh/e1bafa60c61e07a7d70cc2ccaaa81281.js"></script>

![2018-01-20-Hollow-Wall-Fixings.png]({{site.baseurl}}/img/2018-01-20-Hollow-Wall-Fixings.png)

What would you go for?

Well, the metal worm and hollow wall anchors (which I use all the time) are no use in tiles. 
So removing those 2 from the equation results in our final plot:

![2018-01-20-Hollow-Wall-Fixings-Revised.png]({{site.baseurl}}/img/2018-01-20-Hollow-Wall-Fixings-Revised.png)

In the end, I had to buy snap toggles (had never heard of them before), they were much easier to install than spring toggles, which seem to require a lot more than 2 hands at any given moment. 

Early days, but the cabinet is up, feels solid and worth the time spent on it...unlike this post. 

For those new to ggplot2, things to watch out for in the code include adding a border round each point (using "stroke") and later on, scaling the points to the range of the Size variable. 
This is OK because there are only a few points, just don't try it if you've a lot of observations.  
If you check the plots as they evolve you'll also see the impact of the code in removing the legends and labelling the points directly using the ggrepel package.

Until next time..



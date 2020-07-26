---
layout: post
published: true
title: 'A teeny tiny Twitter tale '
subtitle: Use of social media within the NHS to raise awareness of dangerous conditions
date: '2017-11-06'
tags:
  - rstats
  - r-tweet
---

We had an event at work today, bringing together clinicians and QI staff across a range of disciplines and services to provide some education and raise awareness of work that is being undertaken to recognise and treat [Sepsis](https://sepsistrust.org/news/what-is-sepsis/).

We tried to encourage folk to tweet, using the hashtag "#nhshsepsis" (you can also follow our sepsis twitter account on [@nhshsepsis](https://twitter.com/MariaAn92947453) to learn more about this dangerous condition and the work we are doing in Highland).

There weren't a huge amount of tweets, at the time of writing this, but there were enough to justify installing [rtweet](https://cran.r-project.org/web/packages/rtweet/index.html) and taking a look. 

I haven't covered the necessary setup in terms of creating an application - take a look at [https://apps.twitter.com/](https://apps.twitter.com/) and also the [rtweet package help](https://cran.r-project.org/web/packages/rtweet/vignettes/auth.html), you should be able to get yourself going without too much issue. 
The main thing to remember is that you will need to set a callback URL of http://127.0.0.1:1410

Gist below - should all be pretty self explanatory -  looking at the number of tweets, the most favourited, and the most influential (based on likes/RT) tweeter. 

![2017-11-06-most-tweets.png]({{site.baseurl}}/img/2017-11-06-most-tweets.png)


Also a wee look at the devices being used - there's quite a lot of info that can be gleaned from the Twitter API. 

![2017-11-06-devices.png]({{site.baseurl}}/img/2017-11-06-devices.png)



<script src="https://gist.github.com/johnmackintosh/9bfe322ccb270d371e249855a88577c2.js"></script>

There are many functions that I have not even looked at yet, but the key function of simply returning all the tweets with the relevant hashtag was very straightforward. 

With this package, stringr and tidytext, there is a lot that can be done to analyse data from what seems to be the social media channel of choice for the #rstats afficionado

![2017-11-06-most-rt.png]({{site.baseurl}}/img/2017-11-06-most-rt.png)

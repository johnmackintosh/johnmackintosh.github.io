---
layout: post
published: true
title: SIMD Revisited
date: '2020-07-18'
tags:
  - rstats
  - ggplot2
  - hrbrthemes
  - SIMD
subtitle: SIMD data without maps
share-img: /assets/img/20200717_simd_domain_rank_plot.png
---
## The Scottish Index of Multiple Deprivation updated for 2020

I have blogged about the [SIMD](  https://johnmackintosh.com/2017-08-22-simply-mapping/) previously. The last time was using data from 2016. 
Earlier this year, the data was refreshed, and my friend [David Henderson](https://www.davidhen.com/) was hot off the press with some very nice plots indeed. 

Even better, he'd shared his code, so I was tempted to quickly hop onto my laptop and see if I could come up with something new and exciting. I thought it would only take ten minutes, as David had gone through all the pain of the data ingest, but I ended up spending a bit longer than that. 

[Last time round, I produced quite a lot of maps](https://johnmackintosh.com/2017-09-01-easy-maps-with-tmap/), and I didn't see the point in replicating that. In addition, I still haven't sussed out how to embed an interactive map on this Jekyll site, so  I decided to not bother with maps at all. 

I did have a vague idea of what I wanted to do, but I couldn't get it to work. It had been a while since I'd used ggplot2 and I seemed to have forgotten lots. 

So instead, I did what anyone would do in these circumstances, and produced what are essentially scatter plots, but they do look rather nice. 

Another happy accident this time round was my choice of plot theme. 

The last time I worked with SIMD I used theme_ipsum from Bob Rudis's [hrbrthemes]( https://CRAN.R-project.org/package=hrbrthemes)

However, my first attempt did not look quite right, which I think is partly down to how that theme aligns text. It was absolutely brilliant for my maps, but it wasn't quite hitting the target this time round. I'd love to say I had a plan, and was oscillating back and fore between various native ggplot themes, when I chanced on trying out theme_ft_rc from Bob's package.

I think it's lovely:

![20200717_simd_domain_rank_plot.png]({{site.baseurl}}/img/20200717_simd_domain_rank_plot.png)

This is a much condensed image compared to the original, which is so big, I would probably need to get it printed, laminated and stuck on a wall to do it justice. 

This, rather than looking at the overall decile / quintile scores, is showing areas ranked across Access, Crime, Education, Employment, Health, Housing and Income.


Bearing in mind this was done back in January, in the depths of the Highland winter, I'm going to throw in a couple of pics here so you can see what influenced my next plot.

Behold, sunrise in Inverness in Jan 2020: 

![20200717_sunrise2.JPG]({{site.baseurl}}/img/20200717_sunrise2.JPG)


Even better, one Sunday evening we were treated to an amazing sunset, which this pic does not truly capture at all:


![20200717_sunset1.JPG]({{site.baseurl}}/img/20200717_sunset1.JPG)


Behold, theme_sunset:

![20200717_simd_domain_rank_plot_org_blu.png]({{site.baseurl}}/img/20200717_simd_domain_rank_plot_org_blu.png)

I used [ggdark](https://CRAN.R-project.org/package=ggdark) for this one, and did a simple blue to orange scale. 
I know it's nothing like the actual sunset above , but you can't expect me to compete with Mother Nature, even with ggplot2 and its derivatives.


### Save the bees

Beeswarm plots are quite nice I think. 

If you don't agree, look away now:  

Firstly, a parochial view of Highland level data:  

![20200717_simd_domain_Highland_beeswarm.png]({{site.baseurl}}/img/20200717_simd_domain_Highland_beeswarm.png)


Honing in on working age populations -  this is not a great one really, but I'm putting it in anyway:

![20200717_simd_working_age_population.png]({{site.baseurl}}/img/20200717_simd_working_age_population.png)


Now, the whole of Scotland, at Local Authority level:

![20200717_simd_domain_LA_beeswarm.png]({{site.baseurl}}/img/20200717_simd_domain_LA_beeswarm.png)


Look at Edinburgh and Glasgow. (Scotland's 2 main cities, but not necessarily the best :)

They are almost all mirror images of each other, which can be seen more clearly below:

![20200717_simd_domain_big2_rotated_beeswarm.png]({{site.baseurl}}/img/20200717_simd_domain_big2_rotated_beeswarm.png)


Of course, you can't _not_ play around with gganimate :
![20200717_two_cities.gif]({{site.baseurl}}/img/20200717_two_cities.gif)



How would it look if they were combined?

![20200717_two_cities_combined.gif]({{site.baseurl}}/img/20200717_two_cities_combined.gif)


The code is on github [here (me)](https://github.com/johnmackintosh/simd_2020) which is a fork from [David's repo](https://github.com/davidhen/simd_2020).

Thanks again to David, who did all the hard work, and without which, I would not have produced these plots.

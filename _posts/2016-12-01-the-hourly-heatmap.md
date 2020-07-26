---
layout: post
published: true
title: ' The hourly heatmap with ggplot2'
subtitle: a quick follow up to my last post
tags:
  - rstats
  - ggplot2
  - dplyr
date: '2016-12-01'
share-img: johnmackintosh.github.io/img/2016-11-30DailyTemps.png
comments: true
---


I've had a few folk get in touch following my [last post](http://johnmackintosh.com/2016-11-27-calendar-heatmaps/), all commenting on the last plot :  

![2016-11-30DailyTemps.png]({{site.baseurl}}/img/2016-11-30DailyTemps.png)

Most of these enquiries went along the following lines:

"I hadn't thought of doing that. It looks really easy. But how do you do it?"

Unfortunately I was away the last couple of days without access to the code, and I didn't want to pass on the wrong info. 
Back home now though, and the code is now uploaded as a gist to GitHub :

[HeatmapHrbyDay.R](https://gist.github.com/johnmackintosh/520643a1f82a0c7df00cf949ba98a4e9)


What is the use case? Any organisation/operation that is 24/7 will have some sort of data they can monitor via such a  plot. Server loads / web traffic etc could also be analysed this way. 


Some comments :

- The Interpol dataset is pretty large, so we filter out for the first weather station, which conveniently had 2 full years data. We remove all the data we don't need as we go. 

- I use the [ggExtra](https://cran.r-project.org/web/packages/ggExtra/vignettes/ggExtra.html) package for easy removal of gridlines (and depending on the plot, easy rotation of axis labels). I know these are not the main functions or reason for the ggExtra package existing, but these commands are easy to remember.  Big thanks to [Dean Attali](http://deanattali.com/) for all his work on this and other cool R packages.  You don't have to use this package to remove the gridlines, but I will leave you to find the necessary syntax (hint : check my first blog post).

- The beauty and flexibility of ggplot2 is demonstrated once again. The bulk of the plot is produced with these few lines:

~~~~
    p <-ggplot(df,aes(day,hour,fill=temp))+  
    geom_tile(color= "white",size=0.1) +  
    scale_fill_viridis(name=" Hrly Temps C",option ="C")  
    p <-p + facet_grid(year~month)  
~~~~
If you type 'p' in the console at this point you will  see the plot.  The rest of the code is formatting tweaks.
For more heatmap awesomeness, check out Bob Rudis's blog:  
[Bob Rudis faceted heatmaps](https://rud.is/projects/facetedheatmaps.html)


I think I'm done with heatmaps for the time being - hope this helped you visualise your data, would love to hear from you if it did.

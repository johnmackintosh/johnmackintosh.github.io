---
layout: post
published: true
title: Daily metric tracking with ggplot2
share-img: johnmackintosh.github.io/img/2016-MetricCalendarHeatmapWithLabels.png
tags:
  - rstats
  - ggplot2
date: '2016-11-27'
---

In my role I have to produce a lot of run charts and control charts for monthly data, but sometimes there are processes behind the run charts that merit looking at the data on a daily basis. 


It can become impractical to start producing run charts split by day across all metrics (e.g. one for Sunday, one for Monday etc.) so as a quick way of visualising data and spotting patterns for further investigation I'm starting to quite like heatmaps. 

There are a few examples already of calendar style heatmaps in R - specifically Paul Bleicher's [CalendarHeat](http://blog.revolutionanalytics.com/2009/11/charting-time-series-as-calendar-heat-maps-in-r.html) function based on lattice (see link for discussion), and the [ggTimeSeries](https://github.com/Ather-Energy/ggTimeSeries) extension which gives us similar functionality within ggplot2.


Generally, I've seen these used for tracking an individual metric over an extended time period (e.g. a year or more).   I have never got to grips with the lattice system, so I decided to have a bash at producing some thing loosely based on these but using ggplot2 instead.

I had 2 aims:  
- I wanted something that would allow me to visualise data for a larger number of metrics, but over a much smaller time period  
-  It was absolutely not to be a run chart or some other line plot (in my defence, I've produced 1000's of these already and will be producing many more in the future)


Before we go on please bear in mind this is _randomly generated_  data (_from the future!_)  - so it is very much pot luck as to how effective these look.   The aim was to have a mix of high, mid and low frequency variables to mirror typical metrics within most organisations:  
     

###  All metrics, month at a glance


![2016-11-27-Heatmapnolabels.png]({{site.baseurl}}/img/2016-11-27-Heatmapnolabels.png)


This gives a nice overview of a dozen metrics, per day, for a given month, in this case, Dec 2016 - which, at the time of writing, hasn't happened yet (I did say this was made up didn't I?) 

The first question that arises -  to label, or not? Generally heatmaps aren't labelled, and I like the purity of the above plot, but unless it's made available as an interactive plot , the audience has no way to accurately determine the values. 


Speaking of interactive, here is how this would look using plotly within R:

![2016-11-27plotlyheatmap3.png]({{site.baseurl}}/img/2016-11-27plotlyheatmap3.png)

Plotly is really nice, but I can see that people may not want to be hovering over each tile to determine the value, especially if its a plot type that is new to them. 

So, bearing in mind I generally have to export plots as static images, here is a labelled version:  

![2016-11-27-Heatmapwithlabels.png]({{site.baseurl}}/img/2016-11-27-Heatmapwithlabels.png)  

Here I overlaid labels with `geom_text` , after creating separate dataframes to control the text colour - depending on the values within the data you may have to play around with this.  

One criticism of these is that the 0 values are filled and so it becomes difficult to determine much variance between these and the other values for metrics A-C.  

A way round that is to rescale each metric individually to a value between 0 and 1, to standardise the variance in the fill shades across metrics. You then use the rescaled value as the basis for plotting - and overlay with the labels as before:  


![2016-11-27RescaledMonthViewHeatmapWithLabels.png]({{site.baseurl}}/img/2016-11-27RescaledMonthViewHeatmapWithLabels.png)


### Calendar style view by metric


If you want to look at patterns by weekday, both individually and across all the metrics, then this layout can work well. First, here's the labelled, unscaled version:  

![2016-MetricCalendarHeatmapWithLabels.png]({{site.baseurl}}/img/2016-MetricCalendarHeatmapWithLabels.png)

Here is the rescaled version, for comparison:


![2016-11-27RescaledMetricCalendarHeatmapWithLabels.png]({{site.baseurl}}/img/2016-11-27RescaledMetricCalendarHeatmapWithLabels.png)


Bearing in mind this is entirely randomly generated data, you can still see some patterns here for some metrics.  If you try something similar with your own data you may find something that was previously "non-obvious" now stands out. Then you can delve deeper if there is  specific day or sequence that catches your attention.


###  Lose the zeros


This speaks for itself, here is the first plot again but this time I stripped out the zero values so they did not get filled in with the same palette as the other values. 

![2016-11-27-HeatmapWhiteZeroWithLabels.png]({{site.baseurl}}/img/2016-11-27-HeatmapWhiteZeroWithLabels.png)

You may have to denote that white = 0, or it may be assumed that these are missing values instead. Again, the best approach depends on the data and your audience. 

### Summary

Heatmaps may not be everyone's cup of tea. You could use more robust methods to determine whether 2 or more metrics increase together or not. There are also better ways of visualising these relationships, like the awesome looking [corrr](https://drsimonj.svbtle.com/explore-correlations-in-r-with-corrr) package. 

That said, I still like heatmaps as a quick way to visualise high volume/high frequency data underlying some of the high level metrics that I routinely measure. You don't know what previously unseen patterns may emerge, so there is nothing to lose by plotting them. I also find that people who may not be huge fans of charts (there are some strange folk who prefer tables), actually _do_ like heatmaps.  Again, once people get engaged then you can move on to other visualisations in due course. 


I'm going to finish off with a plot I produced earlier in the year, for a talk I did on ggplot2.  This shows data by hour, day, month and year. Its based on a very old dataset from the `Interpol.T` library - I wanted something that would allow me to plot hourly data by day and I have used similar plots with real-life data as well:

![2016-11-30DailyTemps.png]({{site.baseurl}}/img/2016-11-30DailyTemps.png)

It looks better on screen, it has to be said - it got a bit squished during the export process - but you get the idea. I've been trying to recreate something like this with QlikView and that is still very much a work in progress, whereas ggplot2 and lubridate make it easier to take an idea and see it through to reality.


That's all for this post, hope you liked this and hope it has sparked some ideas for taking an alternative look at your data.

---
layout: post
published: true
googlefonts: ["Monoton", "Lobster"]
subtitle: First steps with  time series and prediction in R
tags:
  - rstats
  - forecast
title: 'F is for Forecast '
date: '2016-11-14'
---



At the August meeting of the [Inverness R User Group](https://www.facebook.com/InveRness-R-User-Group-195100587522377/?fref=nf "facebook link"), one of the members gave an introductory presentation on times series in R. One of the data sets we looked at were the weekly CO2 levels from the Mauna Loa Observatory, Hawaii.
This data is provided by the [Scripps CO2 Program](http://scrippsco2.ucsd.edu/).


At the time, the data provided ran up to the end of April 2016. I was interested to see if I could use this data, along with the [forecast](https://cran.r-project.org/web/packages/forecast/index.html) package, to predict levels for a few months into the future, and then do a comparison once the actual levels were known. The  forecast package is just one of the R packages created by Rob Hyndman.  Rob has a ton of great material available - so if you want to learn more about forecasting then you should [investigate his online book](https://www.otexts.org/fpp). 


The data was provided as a .csv file, which I've tidied up a bit and saved to my [github repository](https://github.com/johnmackintosh/F_For_Forecast) as a [text file](https://raw.githubusercontent.com/johnmackintosh/F_For_Forecast/master/ML.txt).  There were 2961 weekly observations at the time. 


### A first look at the data and exploratory plots



Normally, when you first get your hands on data, you want to do some exploring - and this time is no different.
I used ggplot2 to some very quick exploratory charts:

![histrogram]({{site.baseurl}}/img/hist.png)

![]({{site.baseurl}}/img/freq.png)

![dens.png]({{site.baseurl}}/img/dens.png)

![boxplot]({{site.baseurl}}/img/box.png)


So, at first glance, these plots seem to suggest that there is little variation between months, and looking at the density plots or histogram you could probably be quite confident of estimating the bulk of future values would be in the region between 300-390 ppm. 

### The power of plotting data over time



Now for the eye opening part. I spend a LOT of time at work creating an analysing run charts - looking at data displayed over time, and looking for signals of special cause variation in the data. So, with any dataset I'm given, I always plot it in time order. 

Here is the same data as a simple line plot, split by month for each year in the data set:

![dataovertime.png]({{site.baseurl}}/img/dataovertime.png)



This tells a radically different story.  [400 ppm is a critical threshold - and 350 ppm is seen as an ideal safe level.](http://400.350.org/) So I added these threshold lines to the plot (removing the gridlines for less clutter):

![thresholds.png]({{site.baseurl}}/img/thresholds.png)



We can see the threshold has been breached in most months, but I couldn't tell if it had breached in November.  Plus I wanted a sense of just how recently the threshold was crossed, so I filtered the data from 2012:

![thresholds filtered390.png]({{site.baseurl}}/img/thresholds filtered390.png)

 Here I also zoomed in on the y axis and changed the theme to make it easier to distinguish the facets within the plot. It's not very pretty, but it's serviceable, and tells us what we need to know - that the threshold has been crossed within the last few years
 
Here's an alternative look at the data from 2010 as a strip plot. This confirms that, based on the current data, the only months that levels have not breached the threshold are August, September and October: 

![thresholds filtered stripplot.png]({{site.baseurl}}/img/thresholds%20filtered%20stripplot.png)


Finally, I was iterating through some different plots and because its pretty - (but not particularly insightful),  here is an alternative view of the data, again using `geom_jitter`, to produce a scatter plot:  

![seasonal scatter.png]({{site.baseurl}}/img/seasonal%20scatter.png)



### Time to start forecasting


So, the exploratory boxplots, hisograms and density plots are pretty useless as a guide to the future. 
The line plots show the relentless upward trend, throughout the year. 
Will the pattern continue? How high will levels reach?  
That's where the forecast package comes in. 


To take advantage of this package we need our data to be as a time series object, rather than a dataframe. 
We do this using the  `as.ts` command.  


Here's the result - I'm showing the first and last few rows, and the structure of the time series object we created:


![ts structure]({{site.baseurl}}/img/ts.PNG)  


During our R meetup we got this far, and were trying to decompose the data - that is breaking it down into it constituent parts to identify underlying seasonality and trend. I knew the base `decompose` function, but was getting absolutely nowhere with it.

Specifically, I kept seeing this pesky error message:

![peskyerror.PNG]({{site.baseurl}}/img/peskyerror.PNG)


We weren't able to progress much beyond this in the meetup, but at home, with a bit of research I realised that we needed to specify a start time/interval for the time series, and the frequency. That is -  how often is the data being generated - weekly, hourly etc. 

One nice feature in the forecast package is the _`findfrequency`_ function. If you don't know how often your time series data is being generated, or if there might be more than one frequency (e.g. monthly and quarterly components),  then this function will conduct analysis of the data and return the dominant frequency.

As the great Reeves & Mortimer used to say:

![vicnbob.jpg]({{site.baseurl}}/img/vicnbob.jpg)
"We don't know how it works, it just does".

(Actually, its based on spectral analysis of the data - which in very simple terms is a method to break data down into its constituent periods  - a topic beyond the scope of this post - but there is a great guide [here](http://rstudio-pubs-static.s3.amazonaws.com/9428_1197bd003ebd43c49b429f22ea4f36e5.html "spectral analysis time series") -  so if you read that then you **do** know how it works).


There is also a really helpful article on Rob's blog [here](http://robjhyndman.com/hyndsight/seasonal-periods/), to aid you in determining the correct frequency for your data if you want to set it yourself. It's worth trying the _`findfrequency`_    function to verify your choices.  

Calling this function on this data set resulted in a frequency of 50 being specified, and not 52 as might be expected for weekly observations. I then recreated the time series, using this new frequency and providing the start date.   I then used _`start`_  on the time series data to check that the correct year and months had been specified:

![start.PNG]({{site.baseurl}}/img/start.PNG)


**Now** that we have a frequency and start interval, we can use the `decompose` function to break down the time series into the observations, trend, seasonality, and random 'noise'.


Here is the decomposed data:
![decomposed time series plot]({{site.baseurl}}/img/original decomposed.png)

The plot title suggests this is an additive time series - that is the observations are the sum of the seasonal, trend and random observations (so the top plot is the sum of the remaining 3).  A feature of additive time series is that the seasonal and random variations do not change much, regardless of rises/ falls in the  level of the overall trend. 


Its a one-liner to generate a forecast, you can start by simply specifying how many observations into the future you want to predict, along with desired confidence intervals. This assumes you have already specified the frequency - if not you will need to specify that too. There are other advanced options, but these 3 are enough to generate a forecast:

![fcast code.PNG]({{site.baseurl}}/img/fcast code.PNG)

Here I specified a variable (wks_ahead) for the length of the period I wanted to forecast - 21 weeks in this instance to take me from May to the end of September, which, at the time of writing, is the most up to date data available.

This is the original data with the forecast values. 
This was plotted using the `autoplot` function, which is called from ggplot2 (if you have it installed and loaded. Which you should).
Unfortunately  the forecast period is barely visible - look for the blue confidence band either side:

![original forecast.png]({{site.baseurl}}/img/original forecast.png)


Fortunately, we can use additional ggplot syntax to change the theme, and zoom in on the most recent data (from 2016):


![forecastfiltered2016.png]({{site.baseurl}}/img/forecastfiltered2016.png)

This is  better - I specified theme_minimal and amended the range of the y-axis limits but otherwise this is derived straight from the autoplot function. The axis labels etc need tidying up but for an analyst doing iterative exploratory work this is a really useful plot with minimal code required. 

We can now see the distinct confidence bands, and can see that the forecast levels are predicted to remain above 400 throughout the forecast period.

You'll note the chart title indicates which forecasting methods were used to generate the predictions, so let's take a closer look at this. Firstly, the `forecast` function returns an object with a class of "forecast". 
Below you'll see a couple of screenshots from within the RStudio Environment tab showing the structure of the forecast object - here denoted by "fcast":

![fcast list1.PNG]({{site.baseurl}}/img/fcast list1.PNG)


![fcast list2.PNG]({{site.baseurl}}/img/fcast list2.PNG)

Of course you can also call `str(yourforecast)` to see the structure of the object. 


We see that fcast is a list of 10 objects -  it will always contain "model", "method", "mean", "lower", "upper", "level", "x", "residuals" and "fitted".  I won't go into each one but the main thing to know, alongside the model and method, is that "x" is the _original_ time series and **"mean"** is  a time series of the forecast _future_ values. 

I'll come back to this later. Let's take a look at the method:

![fcast method.PNG]({{site.baseurl}}/img/fcast method.PNG)

So what does _this_ mean? Well, the package help tells us that "STL" breaks down a time series into "seasonal, trend and irregular components using loess, acronym STL".

And, again what does _that_ mean? 
Well - I tried to reword the package help, but I think it probably makes more sense to quote it direct:
"_The seasonal component is found by loess smoothing the seasonal sub-series (the series of all January values, ...); if s.window = "periodic" smoothing is effectively replaced by taking the mean. The seasonal values are removed, and the remainder smoothed to find the trend. The overall level is removed from the seasonal component and added to the trend component. This process is iterated a few times. The remainder component is the residuals from the seasonal plus trend fit_".

How about the "ETS(A,A,N)" part?

The ETS part relates to Exponential smoothing, and the additional letters give further information on the error type, trend type, and season type. Here the A relates to "additive", and the N relates to "None", so we have exponential smoothing with additive error, additive trend and no seasonal type.

Some further research is required here, but this is getting out of scope for an introductory post, so check out the websites linked earlier for more information. 

Calling `plot` on the forecast _mean_ will give you a "base R" plot of the forecast values. You can also try `upper` or `lower` to see the confidence limits.


![forecast values base plot.png]({{site.baseurl}}/img/forecast values base plot.png)

Hmmm,  that's not the best looking chart ever is it?


I find ggplot2 a lot easier to deal with than the other R plotting systems. 
It is possible to autoplot this, firstly by assigning autoplot to a variable and then specifying additional plotting commands as before.  Ultimately, I created a new data.frame and tidied the plot up:

![Forecast values by week.png]({{site.baseurl}}/img/Forecast%20values%20by%20week.png)

We know we are projected to remain above 400 PPM through August and September in 2016, for the first time. So what _actually_ happened?

In the last week or so I was able to obtain updated data for the period, reading it directly from the Scripps [website](http://scrippsco2.ucsd.edu/assets/data/atmospheric/stations/in_situ_co2/weekly/weekly_in_situ_co2_mlo.csv "url for weekly data csv").

This is the comparison:



![Forecast v Actual values by week.png]({{site.baseurl}}/img/Forecast%20v%20Actual%20values%20by%20week.png)



Well this looks pretty impressive by any standard. 
Most weeks, the variance appears within + /- 1 PPM,  and very often it's much less.
In fact, here is the variance week on week: 

![variance.png]({{site.baseurl}}/img/variance.png)

This really shows the power of this package, considering there are a host of options and models available, yet with a simple call to a generic function we can get this level of accuracy.

To finish up, I used the summary function in the forecast package to create a dataframe of the forecast values, and the upper and lower limits. Then I added it back to my previous dataframe and assigned the dataframe as a tbl_df.   

One of the side benefits of this is that it wrapped the upper and lower limit column names in backticks - making it easier to refer to these in further plotting and summarising commands. In other words - Hi 90 (the original name, which was causing errors in my ggplot2 code) was surrounded by ` ` , becoming `Hi 90`:

![tibble.PNG]({{site.baseurl}}/img/tibble.PNG)

Here is the final plot, with forecast, actual and confidence levels. 

![Forecast v actual with prediction intervals.png]({{site.baseurl}}/img/Forecast v actual with prediction intervals.png)

When the variance was > -0.5, it was outside the confidence limits. 

Generally though, the forecast was highly accurate.  (There are a range of metrics available to track forecast error, but I won't go into those on this post). 

To wrap up, the main things I took away from this are:

- The original data can be a dataframe,vector etc, but it must be converted using `as.ts` before we can use the package to forecast  

- The need to specify a frequency, and start interval for the time series

- The `findfrequency` function, to assist / check the above  

- The `summary` function can create a dataframe of the forecast values and any specified confidence limits etc.


I'm only scraping the surface here, but I think having a package this powerful, for _FREE_, is amazing, and yet another reason why I love using R so much. I'm looking forward to doing some more testing with this package, and hopefully putting it to good use at work in the future.


 Hope this helped and please get in touch if you would like to see the source code for this post.

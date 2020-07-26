---
layout: post
published: true
title: patientcounter
subtitle: 'fast, flexible census tables'
date: '2020-04-04'
tags:
  - rstats
  - data.table
  - NHS 
---
## Counting patients is hard

Blimey. It's over 3 years since I collaborated with Neil Pettinger on some plots to demonstrate patient flow, using R. What started out as a supposed quick blog post morphed into several weeks of work, a blog post, workshops in Perth and Birmingham, and a much bigger audience than either of us had ever imagined. 

One of the plots that Neil asked about was this one:

![20200404-patient-counter-plot.png]({{site.baseurl}}/img/20200404-patient-counter-plot.png)

This is showing you how many patients are in a ward at any minute. That means counting who has been admitted prior to or during that interval, and who was discharged afterwards, or, not discharged at all. 

It's actually quite tricky, especially to start the plot, where you have to find everyone who was admitted prior to that period.  It took me  quite a while to figure out a method to do it, and it was highly specific. 

## A similar problem

A while back, I got asked to take a look at a spreadsheet by some colleagues in another department. They had the same challenge, they needed to know which patients were in hospital at a certain time each week. Only, it wasn't just one hospital, and it was many patients.  

The spreadsheet hogged my laptop memory like no other document before or since. Every time a filter got changed, hundreds of 'COUNTIF's started whirring away..I was able to come up with a SQL based solution, but if I'd been an 'Excel only' analyst, I'd have been stumped

## Another problem

Slightly more recently, I got a call out of the blue from someone needing help installing R to run a Shiny app. We got that intalled no problem, but the app itself took about 10  minutes to become useable. 
From delving into the code a bit, it appeared to be generating a 24 hour matrix for each day, and looping through patients to count whether they were in or not at each point. 

## How many times can I use the word 'problem'?

I tried to find an alternative solution to this matrix / loop thing. I came across a dplyr row-wise thing, and it seemed to work. But then I saw that row-wise might be getting deprecated, and , long story, short, it was this problem that made me first realise that I needed to learn data.table

## Solution

Behold [patientcounter](https://johnmackintosh.com/patientcounter/index.html). 

A solution that literally no one asked for, but I built it anyway. 
It only has one function, but it's flexible, and will churn out millions of rows in seconds, if your level of granularity requires it.

Although it is primarily aimed at healthcare users, anything that has a time in , and a time out, that might need to be counted (logistics / stock etc?) could be a potential use-case. In fact, if I'd thought about it more, and was aiming for world domination, I'd have called it something more generic.  But, it's done now, and I'm sticking with it. 

There is a wee guide there on the package home page, it's easy enough to use. 

Oh , go on then. Here's some code.

First, installation
```r
install.packages("remotes") # if not already installed
remotes::install_github("johnmackintosh/patientcounter")
library(patientcounter)
```

Now, let's take a look at the inbuilt dataset, which is as fake as a very fake thing indeed.

```r
beds
```

![20200404-beds.PNG]({{site.baseurl}}/img/20200404-beds.PNG)

You can see a typical dataset, but imagine in real life we have hundreds or thousands over a wide range. Most of these _totally fake patients_ are only in for a day or two, whereas our real life data will be a lot more variable, and a lot harder to calculate. 

```r
library(patientcounter)
patient_count <- interval_census(beds, 
identifier = 'patient',
admit = 'start_time', 
discharge = 'end_time', 
group_var = 'bed', 
time_unit = '1 hour', 
results = "patient", 
uniques = TRUE)

head(patient_count)
#>    bed patient          start_time            end_time  interval_beginning
#> 1:   A       1 2020-01-01 09:34:00 2020-01-01 10:34:00 2020-01-01 09:00:00
#> 2:   B       5 2020-01-01 09:45:00 2020-01-01 14:45:00 2020-01-01 09:00:00
#> 3:   A       1 2020-01-01 09:34:00 2020-01-01 10:34:00 2020-01-01 10:00:00
#> 4:   B       5 2020-01-01 09:45:00 2020-01-01 14:45:00 2020-01-01 10:00:00
#> 5:   C       9 2020-01-01 10:05:00 2020-01-01 10:35:00 2020-01-01 10:00:00
#> 6:   A       2 2020-01-01 10:55:00 2020-01-01 11:15:24 2020-01-01 10:00:00
#>           interval_end  base_date base_hour
#> 1: 2020-01-01 10:00:00 2020-01-01         9
#> 2: 2020-01-01 10:00:00 2020-01-01         9
#> 3: 2020-01-01 11:00:00 2020-01-01        10
#> 4: 2020-01-01 11:00:00 2020-01-01        10
#> 5: 2020-01-01 11:00:00 2020-01-01        10
#> 6: 2020-01-01 11:00:00 2020-01-01        10


```

Now, because you're all clever, you'll realise there are no counts here. If you set the results to  'patient' level, then you will get one row per patient per interval. This is very low level granularity, and you can use it to import into another (BI) tool of your choosing, to further aggregate and visualise as you see fit. 

Alternatively, you may want to obtain a summary, grouped or otherwise. 

```r
library(patientcounter)
patient_count_hour <- interval_census(beds, 
identifier = 'patient',
admit = 'start_time', 
discharge = 'end_time', 
group_var = 'bed', 
time_unit = '1 hour', 
results = "total", 
uniques = TRUE)

head(patient_count_hour)
#>     interval_beginning        interval_end  base_date base_hour N
#> 1: 2020-01-01 09:00:00 2020-01-01 10:00:00 2020-01-01         9 2
#> 2: 2020-01-01 10:00:00 2020-01-01 11:00:00 2020-01-01        10 5
#> 3: 2020-01-01 11:00:00 2020-01-01 12:00:00 2020-01-01        11 4
#> 4: 2020-01-01 12:00:00 2020-01-01 13:00:00 2020-01-01        12 3
#> 5: 2020-01-01 13:00:00 2020-01-01 14:00:00 2020-01-01        13 3
#> 6: 2020-01-01 14:00:00 2020-01-01 15:00:00 2020-01-01        14 3

```

Setting results to 'total' gives you a high level summary, or you can set it to 'group' and provide a grouping variable. 

You can shave minutes/ seconds of the start or end of each interval, and , as you can see, it also accounts for patients who do not yet have a discharge date.

I hope it's useful, please try it out, and star on github if you like it etc. 

As always, as with the blog,  feedback is always welcome. 

Cheers, and stay safe.

John

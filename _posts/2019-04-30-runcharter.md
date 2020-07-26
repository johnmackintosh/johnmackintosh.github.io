---
layout: post
published: true
title: runcharter
date: '2019-04-30'
subtitle: A thing what I built
tags:
  - quality improvement
---
## Waffle

I didn't write a blog post in March, because I was busy doing other stuff, but it turns out no one noticed, or maybe they did, and simply didn't care. Well, I care, because it brought my streak of 20 successive monthly blog posts to an end. Which means I need to start a new one. And, if I was going to plot my number of successive blog posts, I might use a run chart.  Which is an absolutely hideous way of shoe-horning in the topic of this month's attempt:

## Substance
After several abortive attempts spanning longer than I care to admit (or remember), I finally got round to building a package for making lots of run-charts all at once : [runcharter](https://www.johnmackintosh.com/runcharter/). 
This is pretty niche, but, at the last count, using the fingers of one hand, I needed *almost all of them* to count the interested parties across the globe. 

What unites us is that we all work in healthcare, we are involved in quality improvement or performance monitoring, and runcharts and SPC charts are a big part of what we do on a daily basis.
Runcharts are easy to build, but are an absolute faff to update when median lines need to be updated. Most people start out building them in a spreadsheet, and adding in additional columns, which is fine for about the first 5 charts, but when you have hundreds, or thousands to analyse, it's simply not viable.

I have done this sort of analysis in SQL, and used various BI tools to attempt it also - [which I blogged about two years ago](https://www.johnmackintosh.com/2017-04-06-the-run-chart-ing-man/). I have a couple of fledgling attempts that were never good enough for release, plus many aborted efforts, so its a relief more than anything else, to cross this off my mental 'to do' list.

What does my package do? It takes a dataframe with 3 columns - a grouping variable, a date, and a y variable representing whatever value it is you want to plot, and it will then analyse the data to find any runs of n successive points on the desired side of the median line. 

If it finds a run, it will calculate a new median, and look for any remaining runs. And it keeps doing this until there are no more runs, or there is no more data, at which point it will generate a lovely faceted plot for you, with a runchart for each individual group (my current record is a 68 panel faceted plot, which, I'm not going to lie,  brought tears of joy to  my eyes). 

Typically, in quality improvement, you are only looking for a run on one particular side of the median line. For example, if you want to reduce patients falling in hospital, then alongside the actual hard work of implementing changes in practice, you would plot the number of recorded falls, and hope to see a sustained reduction below the median line. And then hopefully, with further improvement work, you might see another reduction. 

In my place of work we have seen 3 sustained reductions in reported falls, representing more than a 25% reduction overall from our baseline period. In addition to plotting the overall totals, I also need to be able to analyse individual ward areas - and this package helps me do that. 

I have bowed to popular demand (66% of the people who took an initial interest in the package requested this) and built in the ability to look for runs on both sides of the median, in addition to being able to select either above or below the line. It's not something I would do personally, but I can see why some people want it, so it's there now.

There are a lot of improvements needing made to the code, and I won't be rushing to show it off any time soon, but it works. One thing I really hadn't appreciated when building packages was the namespace / environment issues that crop up when you define a function that can't be found within the scope of another function. I think its fair to say I've hacked my way round that, and this is definitely something that I would love to learn more about. The curse of being the only R user in the organisation, or indeed, possibly the only one around in *(checks notes)* the nearest 100 miles or so.

I also need to get to grips with testthat, which I have failed to do so far. One other change is to make the plot colours editable, as I know how annoying it can be if you find a package useful but hate the plots. 

## Bait and switch
If you work in healthcare/ quality improvement, and you need to make run charts on a regular basis, then you might find it useful.
Other packages also exist, such as the excellent [qicharts2](https://cran.r-project.org/web/packages/qicharts2/index.html). That package uses different rules for analysis, and also produces many other types of chart. 
I also want to highlight [runchart](https://github.com/jsphdms/runchart), which is another package from an R using analyst based in Scotland which is far superior to mine, so be sure to check that out. 

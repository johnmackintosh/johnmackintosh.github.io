---
layout: post
published: true
title: gganimation for the nation
subtitle: gganimate hits CRAN
tags:
  - rstats
  - gganimate
date: '2019-01-06'
---
At the NHS_R conference back in October, I showed a few ways of building animations using gifski - I also wrote up the method I used in an earlier post  [right here](https://www.johnmackintosh.com/2018-11-30-multiple-plots-to-powerpoint/). 

And, the source code for all this stuff is available from my github [here.](https://github.com/johnmackintosh/NHS_R)

However - the gganimate file in the repo is now out of date, so use the code down below. 

Now at the time of the conference, the method I used to build animations involved a fair bit of slicing, dicing and a bit of purrr to build up individual plots and stitch them together. It was a workaround because gganimate was on github at the time, and  installing  packages from github onto NHS computers is likely to be highly problematic, to put it mildly. 

But, the New Year comes, and despite the fact the country is going completely to pot, there are still things to celebrate because gganimate has now hit CRAN.

So, join me as I take my mind off it all by updating some of the code I used with this wonderful new package. 

![20190105-turkey.gif]({{site.baseurl}}/img/20190105-turkey.gif)



Fair Warning - regular reader(s) will have seen these plots before. 
Sorry. 
However, I still think it's (vaguely) worthwhile writing this because while there were / are quite a few examples of how to use gganimate with points, I couldn't find one  that shows how to keep the old points on screen while new points are added (Disclaimer - I haven't put too much effort into additional checking of the latest examples before writing this).

If  you are  using lines and or segments in your plots,  then transition_reveal keeps the history of the line over time, but it did not work with geom_point. 

Here's the setup :

```r
source("1_setup.R")
source("2_data_wrangling.R") 

library(gganimate)

# Change "Tranfer In"  or "Transfer Out" to "Transfer"
plot_data$Movement_Type <- gsub("Transfer.*","Transfer",x = plot_data$Movement_Type)


#convert Movement Type to factor, as the first sequence of dots turns red instead of green
plot_data$Movement_Type <- as_factor(plot_data$Movement_Type)

#check the levels
levels(plot_data$Movement_Type)

plot_data$Movement_Type <- forcats::fct_rev(plot_data$Movement_Type)

levels(plot_data$Movement_Type)

lims <- as.POSIXct(strptime(c("2014-09-03 00:00","2014-09-04 01:00")
                            , format = "%Y-%m-%d %H:%M"))  


p <- ggplot(plot_data,aes(Movement15,Movement_15_SEQNO, colour = Movement_Type)) +
  geom_jitter(width = 0.10) +
  scale_colour_manual(values = plot_colours) +
  facet_grid(Staging_Post~.,switch = "y") +
  # facet_wrap(vars(Staging_Post), ncol = 1) +
  scale_x_datetime(date_labels = "%H:%M",date_breaks = "3 hours",
                   limits = lims,
                   timezone = "GMT",
                   expand = c(0,0)) +
  ggtitle(label = "Anytown General Hospital | Wednesday 3rd September 2014 00:00 to 23:59\n",
          subtitle = "A&E AND INPATIENT ARRIVALS, DEPARTURES AND TRANSFERS") +
  labs(x = NULL, y = NULL,
       caption = "@_johnmackintosh | johnmackintosh.com  Source: Neil Pettinger | @kurtstat | kurtosis.co.uk") +
  theme_minimal(base_size = 11) +
  theme(axis.text.y = element_blank(),
        axis.ticks.y = element_blank()) +
  theme(axis.text.x = element_text(size = 7)) +
  theme(axis.ticks.x = element_blank()) +
  theme(legend.position = "bottom") +
  theme(panel.grid.minor = element_blank(),
        panel.grid.major = element_blank()) +
  theme(strip.text.y = element_text(angle = 180)) +
  guides(color = guide_legend("Movement Type")) +
  ggExtra::removeGrid() 

```

You will need to hit up  my repo to grab the source files, and obviously you'll want to change the titles and labels. 

Now, here is the  gganimate bit:

```r
p <- p +
  labs(title = 'Time: {frame_time}', x = 'Time In/ Out', y = NULL) +
  transition_time(MovementDateTime) + 
  shadow_mark(past = TRUE, future = FALSE) +
  ease_aes('linear') 
p
  
#OR
#animate(p, fps = 5, width = 1000, height = 600)
#save it if you want to:
#anim_save("newgganimate.gif")

```

Transition_time, by itself, plotted each dot where I expected it, but did not keep the previous points on screen. 
I experimented with some other options (ie. shadow_wake and shadow_trail), but it seems 

```r
shadow_mark(past = TRUE, future = FALSE)
```
is the magic sauce for this particular task. 
I could be completely wrong about that, and maybe there is a better way, in which case, let me know in the comments.

Here is the final gif:


![newgganimate.gif]({{site.baseurl}}/img/newgganimate.gif)


Also at the event, I didn't  get a chance to demonstrate the Shiny app I built. 
It was my first ever attempt, and while admittedly it's nothing earth shattering, you have to start somewhere right?
It's over here, but please lower your expectations. There is only one dropdown to interact with:
[https://johnm.shinyapps.io/NHS_R_PatientFlow/](https://johnm.shinyapps.io/NHS_R_PatientFlow/)

And with that, I stop thinking about R for a bit. 




See you next month.

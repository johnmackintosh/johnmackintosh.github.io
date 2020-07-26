---
layout: post
title: 'Chart MakeoveR '
published: true
googlefonts: ["Monoton", "Lobster"] 
subtitle: Working out with (geom) dumbbells
tags:
  - rstats
  - ggalt
  - ggplot2
---

The following chart appeared on my Twitter feed last week: 


![ToothDecayBarChart]({{site.baseurl}}/img/original.png)  

There is quite a lot of information on there, but bar charts are not perhaps the best choice for visualising data over time.    
  
This is not to be critical - it gets a lot of data across in one plot, but I thought it would be interesting to have a bash at displaying the data using R, using my go-to graphics package, ggplot2.    
		
By the way, for those who don't know - the SIMD is the Scottish Index of Multiple Deprivation, which "identifies small area concentrations of multiple deprivation across all of Scotland in a consistent way.  


It allows effective targeting of policies and funding where the aim is to wholly or partly tackle or take account of area concentrations of multiple deprivation". 
(Courtesy of the [Scottish Govt](http://www.gov.scot/Topics/Statistics/SIMD) website). So each postcode in Scotland is assigned to a datazone, and those datazones are ranked on the SIMD - see the link for more information.

  
 
I knew that this would be ideal for faceting (one of the key strengths of ggplot2 - basically creating small multiples, using a couple of different display options, namely geom_grid or geom_wrap).  

I created a simple text file reading the values from the original bar chart, and loaded that up into R. I also created a variable for faceting the plot easily. This required that the labels be re-ordered:  

	    #create facet labels, reset factor levels and display to check order
	    data$facetlabs<- as.factor(paste("SIMD",data$Decile))
	    data$facetlabs<-factor(data$facetlabs,levels(data$facetlabs [c(1,3:9,10,2)])
	    levels(data$facetlabs)  

	
My first draft using ggplot2 resulted in the following:  
  
![Facets.png]({{site.baseurl}}/img/Facets.png)  

Notice the nice subtitle below the main title - this is due to the sterling work of Bob Rudis - who created this functionality and which is now in in the development version of ggplot2 on github. 
For more information on this, please see Bob's blog post [here](https://rud.is/b/2016/03/16/supreme-annotations/).     
   
 
This clearly shows the change over time amongst the deciles. I kept the facet labels  colouring consistent, so as not to distract. In an earlier version, I'd annotated the red target line as such but it didn't look very good when viewing the final plot.  

As a simple alternative, this works quite well. However, I'd seen another blog post by Bob Rudis - (who, you may have gathered, is a ggplot2 / R guru), where he demonstrated his newest creation - dumbbell plots : [exercising-with-ggalt-dumbbells](https://rud.is/b/2016/04/17/ggplot2-exercising-with-ggalt-dumbbells/).   


I wanted to give this a try, so, armed with a fresh install of Bob's package [ggalt](https://github.com/hrbrmstr/ggalt), I produced the following:  

![dumbellvaluessvg]({{site.baseurl}}/img/Rplot.svg)




I'd already managed to get the value labels either side of the dumbell points, but I needed Bob's blog code to get the Year values to appear above the points for SIMD 10.  

Throughout this work, I faced some challenges - probably self inflicted!

For one, I was struggling to filter my "data3" dataframe for a specific SIMD. Then I remembered that this was a factor, not a character. I didn't want to risk breaking the working code elsewhere, and I realised I need to filter on the factor levels, but could not get the correct syntax.  

Thankfully, Stack Overflow came to the rescue, with a solution that I would probably not have arrived at myself:
[filter-factor-levels-in-r-using-dplyr](http://stackoverflow.com/questions/30052042/filter-factor-levels-in-r-using-dplyr).  

Armed with this knowledge I created another small dataframe and used that for the filtering and subsequent labelling:

    nonWantedLevels<-c(1:9)
    SIMD10<- filter(data3,!as.integer(dfy)%in% nonWantedLevels)


This may not be the most efficient way of doing this, but it worked..   

The other wee challenge towards the end was to figure out the use of vjust to get the year labels above the SIMD 10 dumbell.  

A quick check on [Stack Overflow](http://stackoverflow.com/questions/7263849/what-do-hjust-and-vjust-do-when-making-a-plot-using-ggplot) suggests that acceptable values are in the range 0 and 1.  

I need to do some more research on this because entering 1  for the vjust did not put the labels where I wanted them - here is my final code for the plot:     

    # text above points
    p <- p + geom_text(data=SIMD10,
                         aes(x=dfx, y=dfy, label="2009"),
                        size=3, vjust=-1.5, fontface="bold")
    p <- p + geom_text(data=SIMD10,
                         aes(x=dfxend, y=dfy, label="2015"),
                         size=3, vjust=-1.5, fontface="bold")




Geom_text is not one I've had to use much, if at all to date so now I have a better handle on it (apart from vjust, obviously) I may make more use of it at work to annotate run charts. 

One feature missing from these is the ability to show the 60% target by 2010. It would be possible to add in a vertical marker at the corresponding point on the x-axis to represent the 2010 value.

However, as it stands this does a good job of allowing us to see and compare relative progress from each decile's starting point.    

Its also easier to see the progress in SIMD 7 compared to SIMD 8.

This is quite a simple example, and data visualisation purists may have arguments against using these plots.

I think they are a useful tool to have available - sometimes, you have to break the data vis rules to get people's attention.

From  there you can have the discussions you need to allow them to probe and develop their understanding, and then over time, you can introduce the "correct" charts.  

The faceted plot is better suited, and gets all the data accross, but if geom_dumbell becomes mainstream in ggplot2, then I certainly wouldn't rule out using it again.

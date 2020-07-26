---
layout: post
published: true
title: let there be progress
subtitle: The 'wrapr' package for use with dplyr programming
date: '2017-06-01'
tags:
  - rstats
  - dplyr
  - wrapr
---

I'm the first to admit I'm not an R expert, (even duffers can blog about it though), but when I began thinking about writing some dplyr functions to help me create and analyse [run charts](http://johnmackintosh.com/2017-04-06-the-run-chart-ing-man/), I had no idea that I was going to struggle quite so much getting to grips with Non Standard Evaluation and Lazy Evaluation (see my last post for links and more background).

To clarify, I wanted to create flexible dplyr functions, without hardcoded grouping parameters, so I could query different SQL Server tables and apply the same functions reliably to transform and plot the data. 

Eventually I sussed things out and created the functions I needed, but it was yet another example of how R makes me feel really stupid on a regular basis. 
Please tell me I'm not alone with that...  

I'm aware that dplyr 0.6 is **imminent**,  and that my code will need revising as the dplyr verbs with underscores (for programmatic use) will be deprecated (at least eventually). 

While I could probably install the dev version , I thought I should finally take a look at the let function in the [wrapr](https://CRAN.R-project.org/package=wrapr) package, as I'd seen numerous tweets giving it [positive feedback](https://twitter.com/jsonbecker/status/852339824774262784).

I had a bash at recreating one of my functions - got some help directly from [win-vector](http://www.win-vector.com/site/) and was then able to rewrite all my remaining functions quickly. 

I've put the code gist [here](https://gist.github.com/johnmackintosh/30ec559bac2d09be4d0d01122b5dd35f) - should hopefully run as intended, assuming you have the current dplyr 0.5 and wrapr packages installed.


## Points to note  

- I commented out a line in the wrapr let example code for this demo so that all 3 return the same output.   

- There is some nice code to handle the lack of a grouping column, courtesy of Win-Vector. For my real world use, there will always be a grouping variable, but its nice to build this in for future requirements.

- I used slice instead of filter in the NSE code - I could not get it to work at all otherwise. I'm pretty convinced I've done something wrong here, because the function only runs if I don't wrap the variables in quotes when I call it. For the life of me I couldn't figure out why a grouping call would work in one NSE function and fail in another when the same syntax was being used. Bearing in mind that the work I actually WANTED to do was going to be complex enough, I was just happy that it worked. 

## Code examples  

Day to day, interactive dplyr code :  
![2017-05-31 regular interactive dplyr.PNG]({{site.baseurl}}/img/2017-05-31%20regular%20interactive%20dplyr.PNG)    

My attempt at the whole NSE/lazy eval thing :
![2017-05-31 dplyr original NSE.PNG]({{site.baseurl}}/img/2017-05-31%20dplyr%20original%20NSE.PNG)   

Wrapr code :  
![2017-05-31 wrapr comparison.PNG]({{site.baseurl}}/img/2017-05-31%20wrapr%20comparison.PNG)  

With the let function you create the mapping block first, then use the variables there in your function - and can use the regular dplyr verbs. Once I'd seen how to set up the code, it was easy to recreate the others.

Outputs (in same order as above code examples):   
 

![2017-05-31 interactivePNG.PNG]({{site.baseurl}}/img/2017-05-31%20interactivePNG.PNG)  

  

![2017-05-31 NSE output.PNG]({{site.baseurl}}/img/2017-05-31%20NSE%20output.PNG)  




![2017-05-31 wrapr output.PNG]({{site.baseurl}}/img/2017-05-31%20wrapr%20output.PNG)   

Here's another example of using the underscore versions compared to wrapr. 

This time the NSE code works with quoted variables:  

![2017-05-31 NSE StartDate.PNG]({{site.baseurl}}/img/2017-05-31%20NSE%20StartDate.PNG)

And here is the wrapr equivalent using let:  

![2017-05-31 wrapr start date.PNG]({{site.baseurl}}/img/2017-05-31%20wrapr%20start%20date.PNG)  


From a real world deadlines / getting things done perspective - if you need to create flexible dplyr functions, and you need to do it soon - take a look at wrapr to see if it fits your needs.

---
layout: post
published: true
title: Simply Mapping
subtitle: Easy mapping with Simple Features
gh-repo: johnmackintosh/SIMD_2016-R  
gh-badge: [star, fork]  
tags:
  - rstats
  - simple features
  - mapping
date: '2017-08-22'
---
## First attempts with simple features

The latest edition of the [Scottish Index of Multiple Deprivation](http://www.gov.scot/Topics/Statistics/SIMD) (SIMD) was released last year, and has been getting a bit more promotion recently as part of Scotland's open data - [for example, an R package has been made available to calculate the SIMD rankings](http://www.gov.scot/Topics/Statistics/SIMD/analysis/openSIMD).

The purpose of the SIMD is that it  "identifies small area concentrations of multiple deprivation across all of Scotland in a consistent way. It allows effective targeting of policies and funding where the aim is to wholly or partly tackle or take account of area concentrations of multiple deprivation.

SIMD ranks small areas (called data zones) from most deprived (ranked 1) to least deprived (ranked 6,976). People using SIMD will often focus on the data zones below a certain rank, for example, the 5%, 10%, 15% or 20% most deprived data zones in Scotland.

SIMD provides a wealth of information to help improve the understanding about the outcomes and circumstances of people living in the most deprived areas in Scotland" ( from the main website linked above). 

To put this into more practical terms, each Local Authority area in Scotland is divided into DataZones, with a roughly equal population in each. Each zone is ranked (low to high, higher is less deprived) on a variety of metrics covering health, education, access to services, housing etc. Various spreadsheets, database files and shape files are now available for analysis. 

There are already some great examples of mapping online, including a fantastic interactive map, but I wanted to try mapping it myself using the ["sf"](https://cran.r-project.org/package=sf) package. 

If you want to follow along, a couple of things you need to bear in mind:

1. You'll need to download shapefiles from [this link](http://sedsh127.sedsh.gov.uk/Atom_data/ScotGov/ZippedShapefiles/SG_SIMD_2016.zip) on the [SIMD page](http://www.gov.scot/Topics/Statistics/SIMD)  
2. You'll need to install the dev version of ggplot2 in order to benefit from geom_sf().   
This will involve devtools, and on Windows, RTools in order to download and install the package successfully.  
First install the devtools package from your usual package repository, and then use "devtools::install_github("tidyverse/ggplot2")" to install the development version.

```r
library(sf)
library(dplyr)
devtools::install_github("tidyverse/ggplot2")
library(stringr) 
library(viridis)
library(hrbrthemes)# optional - but awesome and highly recommended
library(extrafont)# optional, might be required if you are a windows user and want to use hrbrthemes
```   


You'll see I'm using [hrbrthemes](https://cran.r-project.org/package=hrbrthemes) - which is **very, very nice indeed**.  You are free to use other themes of course, but if you haven't  at least tried hrbrthemes then do yourself a favour. This was the first time I'd used it and it's definitely my default for the foreseeable future.

I'm also using extrafont - for Windows users, you may find errors relating to missing fonts when trying hrbrthemes and other custom themes without this packaged being installed. 
It makes it easier to add additional fonts to your system. 
Install the package, this will install "extrafontdb" - and follow the help to register the fonts on your system - this takes a while initially but worth doing (it's just one command - font_import() -so not too onerous). 

Right - to the actual mapping.  

First, read in the shape file:


```r
scot <- st_read("SG_SIMD_2016.shp") 
```

This results in  a dataframe and sf object incorporating 6976 observations and 50 variables. 

Then, for simplicity, I'm going to change all the variables to lowercase, and create a smaller dataframe for all the observations relating to the Highland region ( by filtering on 'laname'):

```r
colnames(scot) <- colnames(scot) %>% str_to_lower()
highland <- filter(scot, laname == "Highland")
```

The code below will give a map of the Highland region coloured by quintile ( that is, a score of 1-5, with 1 indicating the most deprived areas and 5 the least):

```r
  ggplot(highland) +
  geom_sf(aes(fill = quintile)) +
  scale_fill_viridis("quintile",option = "C",
                     guide = guide_legend(title = "Quintile")) +
  ggtitle("SIMD 2016 - Highland Council Area by Quintile",
          subtitle = "1 = most deprived, 5 = least deprived") +
  theme_ipsum(base_size = 10) +
  theme(plot.title = element_text(hjust = 0))
  ```
  
  Here is the resulting plot:
  
  ![SIMD2016_quintile.png]({{site.baseurl}}/img/SIMD2016_quintile.png)

  
 

  
There are quite a few interesting variables within the dataset.  The SIMD scores are available by quintile, decile and vigintile, plus overall all rankings by domains such as health and education, and the main SIMD ranking where 1 is the most deprived and 6976  is the least deprived datazone (in Renfrewshire, and East Renfrewshire respectively, if you're interested). 

Here are some of the other plots I produced - the code being remarkably similar to the above example in all cases:

Scotland by quintile: 

```r
ggplot(scot) +
  geom_sf(aes(fill = quintile)) +
  scale_fill_viridis("quintile",option = "C") +
  ggtitle("SIMD 2016 - Scotland by Quintile",
          subtitle = "1 = most deprived, 5 = least deprived") +
  theme_ipsum(base_size = 10) 
```


![scotland by quintile.png]({{site.baseurl}}/img/scotland by quintile.png)

The problem with this plot is that quite a large chunk of the central belt does not appear to have rendered correctly - too much absence of colour. This is because of the density of the datazones and the thickness of the polygon lines.

So let's have a look at Edinburgh and Glasgow, the 2 main cities, using a different viridis palette:


![Edinburgh deprivation by quintile]({{site.baseurl}}/img/SIMD2016Edin_quintile.png)


![glasgow deprivation by quintile]({{site.baseurl}}/img/SIMD2016Glasgow_quintile.png)


I mentioned that the area around Renfrew had the lowest and highest ranked datazones, so let's take a look at them:

```r
 renfrew <- filter(scot,laname %in% c("East Renfrewshire","Renfrewshire"))
 ggplot(renfrew) +
  geom_sf(aes(fill = quintile)) +
  scale_fill_viridis("quintile",option = "C",
                     guide = guide_legend(title = "Quintile")) +
  ggtitle("SIMD 2016 - Renfrewshire & East Renfrewshire by Quintile",
          subtitle = "1 = most deprived, 5 = least deprived") +
  theme_ipsum(base_size = 10) +
  facet_wrap(~laname)
  ```
  
  ![renfrew faceted]({{site.baseurl}}/img/SIMD2016_Renfreshire_quintile.png)

Here is the unfaceted version:

![unfaceted renfrew]({{site.baseurl}}/img/SIMD2016_unfaceted_Renfreshire_quintile.png)

## Give me fuel, give me fire, reduced deprivation's my desire


A focus on the Highlands now. Amongst the many variables are some relating to the availability of central heating within homes, and average drive times to access local services, such as retail, GP's, and petrol stations:

![highland-central-heating.png]({{site.baseurl}}/img/highland-central-heating.png)

I'd imagine a lot of older houses in the Highlands still have open fireplaces providing plenty heat and water, but which may not qualify as "central heating" compared to a conventional boiler / oil fired system. 


![average drive time to petrol station]({{site.baseurl}}/img/highland-drive-time-petrol.png)

Again, I can see this being quite a meaningful metric in more densely populated area, but this is less meaningful in Highlands.  You don't just get on the road and drive in a straight line to your destination, we have hills,  moors, and scenic winding roads that you can't drive very quickly on.  And of course there are fewer filling stations generally in the less populated areas. 


![highland depression]({{site.baseurl}}/img/SIMD2016_depression-percent.png)

I've also plotted other areas within Scotland and these results are not out of the ordinary -all the info is available to you within the dataframe, so you can try other areas to check.


Finally, the overall highland rankings - remember higher rankings mean that the area is less deprived: 

![highland overall rankings]({{site.baseurl}}/img/highland-rank.png) 


Main takeaway - the 'sf' package makes mapping pretty easy - if you can get your hands on a shapefile then you should be able to import it and plot it without too much difficulty, and the dev version of ggplot2 makes it even simpler.

[Code and links to data sources on GitHub](https://github.com/johnmackintosh/SIMD_2016-R) 

[ All source data from: www.gov.scot/Topics/Statistics/SIMD](http://www.gov.scot/Topics/Statistics/SIMD)

[Data Source License: www.nationalarchives.gov.uk/doc/open-government-licence/version/3](http://www.nationalarchives.gov.uk/doc/open-government-licence/version/3/)

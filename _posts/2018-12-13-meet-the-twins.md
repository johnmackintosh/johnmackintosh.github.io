---
layout: post
published: false
title: Twins on the up
date: '2018-12-13'
tags:
  - rstats
  - tidyr
  - readxl
share-img:  johnmackintosh.github.io/img/arnie.jpg
---

## Are multiple births on the increase?


My twin boys turned 5 years old today. Wow, time flies. Life is never dull, because twins are still seen as something of a novelty, so wherever we go, we find ourselves in conversation with strangers, who are intrigued by the whole thing. 

In order to save time if we ever meet, here's some FAQ's:

 - No, they're not identical
 - Yes, I'm sure
 - No, they do not have similar personalities
 - They like different things - One likes Hulk and Gekko, the other likes Iron Man and Catboy. 

Recently I've been hearing and seeing anecdotal evidence that twins and multiple births are on the increase.  I tried to find some data for Scotland, and while there is a [lot of information on births in Scotland available](https://www.opendata.nhs.scot/dataset/births-in-scottish-hospitals), I couldn't find breakdowns of multiple births. 
However, I did find some information for England and Wales, so let's look at that. 

In this next bit, they key thing that may be of interest is the use of tidyr::gather.
There has been some discussion on #rstats Twitter about things people struggle with and a surprising amount of people struggle to remember the syntax for tidyr's gather and spread.

(I can neither confirm or deny I am one of them). 

The data was found [here](https://www.ons.gov.uk/peoplepopulationandcommunity/birthsdeathsandmarriages/livebirths/datasets/birthcharacteristicsinenglandandwales)


```r
library(readxl)
library(dplyr)
library(tidyr)
library(ggplot2)

data <- read_xls("birthcharacteristicsworkbook2016.xls", sheet = "Table 11",
                 range = "A10:I87")

data <- data %>% 
  rename(Year = X__1,
         All_ages = `All maternities with multiple births`,
         Under20  = X__2,
         `20_to_24` = X__3,
         `25_to_29` = X__4,
         `30_to_34` = X__5,
         `35_to_39` = X__6,
         `40_to_44` = X__7,
         `45_and_over` = X__8)

# the 1981 data is borked, so ignore that

```
Note use of gather to combine all the age groups into an age_group variable.

We use the Year column as an index so we have an entry for every age group, for every year, with the value represented as 'maternities'.

Back to the code:

```r
long_data <- data %>% 
  filter(Year != "1981") %>% 
  gather(key = age_group, value = "maternities", -Year)

long_data$Year <- as.numeric(long_data$Year)
long_data$age_group <- forcats::as_factor(long_data$age_group)
long_data$maternities <- as.numeric(long_data$maternities)


ggplot(long_data,aes(Year, maternities), group = age_group) +
  geom_line() +
  geom_point() +
  facet_wrap(vars(age_group), scales = "free_y") +
  ggtitle(label = "England and Wales maternities with multiple births - numbers",
          subtitle = "By age of mother, 1940 to 2016") +
  labs(x = NULL, y = "Multiple maternities")

```

![2018-12-13-counts-common.png]({{site.baseurl}}/img/2018-12-13-counts-common.png)


![2018-12-13-counts-free.png]({{site.baseurl}}/img/2018-12-13-counts-free.png)


```r
# Let's do rates

rates <- read_xls("birthcharacteristicsworkbook2016.xls", sheet = "Table 11",
                 range = "A89:I166")

rates <- rates %>% 
  rename(Year = X__1,
         All_ages = `All maternities with multiple births per 1,000 all maternities`,
         Under20  = X__2,
         `20_to_24` = X__3,
         `25_to_29` = X__4,
         `30_to_34` = X__5,
         `35_to_39` = X__6,
         `40_to_44` = X__7,
         `45_and_over` = X__8)

long_rates <- rates %>% 
  filter(Year != 1981) %>% 
  gather(key = age_group, value = "multiple_maternities_per_1000", -Year)

long_rates$Year <- as.numeric(long_rates$Year)
long_rates$age_group <- forcats::as_factor(long_rates$age_group)
long_rates$multiple_maternities_per_1000 <- as.numeric(long_rates$multiple_maternities_per_1000)

ggplot(long_rates,aes(Year, multiple_maternities_per_1000), group = age_group) +
  geom_line() +
  geom_point() +
  facet_wrap(vars(age_group)) +
  ggtitle(label = "England and Wales Rate of maternities with multiple births  - per 1,000 all maternities ",
          subtitle = "By age of mother, 1940 to 2016") +
  labs(x = NULL, y = "Multiple maternities")

```

When we look at maternities with multiple births as a rate per 1000 maternities, we see the increase in multiple births among older mothers, especially in the over 45 group.
![2018-12-13-rates-common.png]({{site.baseurl}}/img/2018-12-13-rates-common.png)

Again, with free scales on the y axis - which helps us see almost all age groups are exhibiting an increase - compare the 20-24 age group as a rate and as count for example. 

![2018-12-13-rates-free.png]({{site.baseurl}}/img/2018-12-13-rates-free.png)

Looks to me that overall, the rate of multiple births is increasing. 
What's driving this? 
Can it continue?
Will people ever stop asking us if the twins are identical?

![arnie.jpg]({{site.baseurl}}/img/arnie.jpg)





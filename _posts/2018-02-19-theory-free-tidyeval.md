---
layout: post
published: true
title: Theory free tidyeval
date: '2018-02-19'
tags:
  - rstats
  - tidy evaluation
  - dplyr
  - rlang
subtitle: Getting our feet wet with dplyr programming
---
 

I've been putting off trying to suss out  dplyr's tidy evaluation for a long time, but I've been experimenting a bit with it so just making some notes here. 
It's hard to talk about tidy evaluation / Non Standard Evaluation without getting into a whole load of theory, but I'm going to try it anyway, for the sake of other people like myself who don't use R all the time.

There are people way smarter than me who've written great articles on tidy evaluation, see [Mara's](https://maraaverick.rbind.io/) resource list [here.](https://maraaverick.rbind.io/2017/08/tidyeval-resource-roundup/) You should definitely check those out for more in-depth understanding, if you want to really get under the hood. 

Personally, I thought the original programming with dplyr vignette started off OK, but quickly went from " a pleasant paddle in the sea" to "here be krakens". 

So my aim is to keep this firmly in the range where you can take your flip flops off and roll up your trouser legs, without getting soaked. 

Disclaimer - there are things I know about, and there are things I don't know too much about - and this is in the latter camp. If you see anything wildly innaccurate or misleading, please let me know.. thanks :)

What's the deal with NSE? 
Basically, it allows you to create flexible dplyr functions, to operate on column names which you don't know yet know the names of. 

There are other reasons too - but for me, it the "write once, use many times" flexibility that is a key strength. 

Most of the easier examples you see will show you how to group by a variable, and perhaps mutate a new column using another varible. 


I'm using the mtcars and flights datasets, which most (if not all) R users will have access to show a few examples. 

These mirror the sort of thing I need to do in my work -  they might not be massively appropriate for the mtcars and flights data - but you can try them, see what they do, and adapt to your data.


# Grouping on one variable, filtering, mutating, then arranging by multiple variables

```r
library(dplyr)

new_median_rows <- function(df,groupvar = NULL, x = NULL, nrows = 5, ...) {
  sortvars <- quos(...)
  x_quo <- enquo(x)
  groupvar_quo <- enquo(groupvar)
  
  df %>%
    
    arrange(!!!sortvars) %>%
    group_by(!!groupvar_quo) %>%
    filter(row_number(!!x_quo) %in% 1:nrows) %>%
    mutate(Baseline = median(!!x_quo))
}

```

Let's test it out : 

```r
test <- new_median_rows(df = mtcars,
                        groupvar = cyl,
                        x = mpg,
                        nrows = 5,
                        gear, carb)

```

The good news is, this works (or at least, we get some output instead of a bunch of error messages)

What is this function even doing?

We want to group a dataframe by a variable, grab the first  5 rows in each group, calculate a new variable ( taking the median value of mpg, in this example), and then we want to sort the output by 2 further variables.

Let start with the easy bits. 
I am simplifying things here probably, but if you are doing any sort of operation with a single variable, then you need to :

- define it in the function using "enquo"
- then bang bang it (using !!)

Why? well that's getting into theory and we don't want to get our trousers wet. 
Oh OK then - it has to do with quoting and unquoting the variables, in order that you can pass "bare" variable names to your function.
(which is why we can write x = mpg, and not x = "mpg" when we actually try it out).

Just accept that you will need to do this, hence my x variable becomes:
x_quo <- enquo(x)
then 
median(!!x_quo) in the later part of the function

The same applies when grouping using the "groupvar_quo" variable, and also filtering out the first few rows.

What about those "sortvars".  
What are they? They're not defined in the function. 
Actually - they are - in those 3 little dots at the end. 

So when we tested the function with 

```r
test <- new_median_rows(df = mtcars,
                        groupvar = cyl,
                        x = mpg,
                        nrows = 5,
                        gear, carb)
```
We asigned variables to all the named arguments,and the gear and carb - which look like they are hanging around at the back and up to no good, are actually passed to sortvars. 


You can't bang bang those though, they need ,er , tripple bangs? (Is that a thing?)
Anyway - you need to use 3 exclamation marks.


## super simple takeaway

one variable = !!  

2 or more variables = ... ,  which in turn requires !!!  


There are a few more examples below, they are all variations on the same theme, using most of the dplyr verbs. 


```r
new_median_rows3 <- function(df,sortvar = NULL, x = NULL, nrows = 5, ...) {
  groupvars <- quos(...)
  x_quo <- enquo(x)
  sortvar_quo <- enquo(sortvar)
  
  df %>%
    
    group_by(!!!groupvars) %>%
    filter(row_number(!!x_quo) %in% 1:nrows) %>%
    mutate(Baseline = median(!!x_quo))
}

```

```r
flight_test2 <- new_median_rows3(flights,sortvar = dep_time,
                                 x = arr_delay,
                                 nrows = 3,
                                 origin,carrier)
```

Again - these are perhaps not the best data manipulation examples for these particular datasets but you should be able to suss out what's going on and adapt these to your needs:

<script src="https://gist.github.com/johnmackintosh/3b8e6f0a4a61043ee04f7211b956d345.js"></script>

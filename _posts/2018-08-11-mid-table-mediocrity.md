---
layout: post
published: true
title: 'Mid Table Mediocrity '
date: '2018-08-11'
subtitle: Visualising potential winners in the Scottish Championship
tags:
  - rstats
  - ggplot2
  - purrr
  - ggalt
---
Last August, I was given a surprise gift of a season ticket to watch my local football team, Inverness Caledonian Thistle. 

They'd been relegated from the top league in Scotland the season before, and needed all the local support they could get. In truth, the gift ( from my father) was a sneaky attempt to get my eldest son hooked into supporting his local team (as every adult season ticket allows free entry for an under-12). 

Long story short, by the end of the season, it was me that was hooked. After an appalling start to the season, the team went on a couple of great runs of results.  In the late Autum, from languishing in 9th position (out of 10 teams) they went on an unbeaten run which saw them break their record for the number of games without conceding a goal. 

Then, towards the end of the season, they went on another unbeaten run, featuring a number of consecutive wins. 
This saw supporters start to dream of finishing in 4th place, and with it, a chance of promotion back to the Premier League. 

The winners automatically get promoted, with 4th playing 3rd, then the winners playing the 2nd team, and the winners of THAT game, playing the second bottom team from the premier league.  

So the hope was that ICT would finish 4th, and somehow navigate their way through 3 more matches against teams who finished higher in the league or were in the league above. 

The league standings, prior to the latest round of matches, were as follows:

![2018-08-11-current.png]({{site.baseurl}}/img/2018-08-11-current.png)


St Mirren were runaway leaders, and heading for automatic promotion. Livingston looked to have cemented 2nd place. At the other end, Brechin were doomed to be relegated, with Dumbarton also looking at a playoff to remain in the league. This left 6 teams scrapping for 3rd and 4th. The teams were reasonably evently matched - on any given day, any team could beat anyone else. 

By the way, QOTS is my abbreviation for Queen of the South, a  team based in Dumfries, in the Scottish Borders, and not to be confused with Queens of the Stone Age, who are a hard rock group from California. 
Although I'm pretty sure QOTSA frontman Josh Homme could do a job up front for any team as he's a big lad. 

Anyway - I started to think about ways to plot potential placings on a game by game basis. I wasn't concerned with thinking about correct scores or goal difference. It was easy enought to calculate where one team might be in the table based on 3 points for a win, 1 for a draw and none for a loss. But then there were all the other games to take into account too. 

A base R command that I have used in plotting in the past is expand.grid(). 
It's especially useful for plotting heatmaps to ensure that there is an observation for every combination of the x and y axis. 
It's also really useful for working out possible combinations of football results.

![2018-08-11-setup.PNG]({{site.baseurl}}/img/2018-08-11-setup.PNG)

So with the above fixture list, and current league placings, we can start to figure out the potential outcomes. 
```r
first <- head(fixtures,1)

permutations <- expand.grid(first$home,first$away,status,Results)
permutations
```

![2018-08-11-permutations-trial.PNG]({{site.baseurl}}/img/2018-08-11-permutations-trial.PNG)


That seemed to work, so let's create a function:

```r
get_combo <- function(x,y){
  permutations <- expand.grid(x,y, status,Results) %>% 
    rename(home = Var1, away = Var2,status = Var3, results = Var4) %>% 
    mutate(home = as.character(home),
           away = as.character(away),
           results = as.character(results))
  return(permutations)
} 

```

Now we can use purrr to apply the function to each fixure. This returns a list of dataframes, which can be combined with bind_rows, and then we can left join to another ancillary table to return the potential points gained for each combination of result:


```r
permutations <- map2(fixtures$home,fixtures$away, get_combo)

permutations <-  bind_rows(permutations) %>% 
  left_join(lookup, by = "results")

permutations
```

![2018-08-11-permutations-function-output.PNG]({{site.baseurl}}/img/2018-08-11-permutations-function-output.PNG)

We can then take these outcomes, and with our current points totals, start to work out possible points for each team for each combination. 

And of course we can visualise those too:

![2018-08-11-potential-standings.png]({{site.baseurl}}/img/2018-08-11-potential-standings.png)

That's a start, but, a lot of these teams have similar colours and its hard to work out who they are. 
Perhaps a dumbbell plot might help? We can use the ggalt package for that:

```r
p <- ggplot(potential_summary,aes(y = reorder(team,xstart), x = xstart,xend = xend,
                             group = team)) + 
  geom_dumbbell(size = 1.25,aes(colour =  team)) +
  scale_colour_manual(values = finalcolours) +
  theme_minimal() +   
  ggtitle(label = "Potential Points - Scottish Championship",
          subtitle = "Range of possible points after games played 14th Apr 2018") +
  labs(y = "Team", x = "Possible Points") +
  theme(legend.position = "bottom") 
```

![2018-08-11-dumbell-v1.png]({{site.baseurl}}/img/2018-08-11-dumbell-v1.png)

Hmm, let's zoom in a bit on our mid table teams. We can do that by using coord_cartesian(). 

And while we're at it, let's add some labels on the dumbbells, and remove some of the extraneous axis labels and tick marks:


```r

##zoom in on mid table

q <- p + coord_cartesian(xlim = c(35,56)) +  
  scale_x_continuous(limits = c(35,56),breaks = c(35:56)) +
  ggtitle(label = "Mid Table Mediocrity in the Scottish Championship",
          subtitle = "Range of possible points for teams who are \n outside promotion or relegation places") +
  labs(caption = "games to be played 14th Apr 2018")
q


q <- q + geom_text(aes(xstart - 1,label = xstart),size = 3)
q <- q + geom_text(aes(xend  + 1,label = xend),size = 3)
q <- q + ggrepel::geom_text_repel(aes(xstart + 2,label = team),size = 3)
q
# get rid of labels etc. that we don't need

q <- q + coord_cartesian(ylim = c(3,8)) +
  theme(legend.position = "none") +
  theme(axis.title.x = element_blank(),
        axis.text.x = element_blank(),
        axis.ticks.x = element_blank()) +
  theme(axis.title.y = element_blank(),
        axis.text.y = element_blank(),
        axis.ticks.y = element_blank()) +
  ggExtra::removeGrid()

q
```


![2018-08-11-mid-table-dumbbell.png]({{site.baseurl}}/img/2018-08-11-mid-table-dumbbell.png)

So now we can see that Inverness, Morton and Queen of the South are really closely bunched up, but with a bit to do to make into 4th. They certainly won't mangage to get into those positions based on the next match. 

We can split out the potential placings for each team, based on whether they win, lose or draw. 

![208-08-11-faceted-potential-placings.png]({{site.baseurl}}/img/208-08-11-faceted-potential-placings.png)

Or we can just show everything on one plot, and let the reader try and make sense of the resulting stramash:

![2018-08-11-stramash.png]({{site.baseurl}}/img/2018-08-11-stramash.png)

(NB - I edited this plot slightly, compared to the code in the gist below - mainly getting rid of the x-axis labels and tick marks as they are redundant due to the labelling on the points themselves).


Here's the full gist if you want to mess around with it:
<script src="https://gist.github.com/johnmackintosh/956e7b9f9adf13ca77518adf75f98a99.js"></script>

There are a few more combinations you could try, plus some more work with tidyr might allow you to create different (and better plots)..

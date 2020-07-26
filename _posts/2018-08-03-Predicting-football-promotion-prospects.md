Untitled
================

Setup current fixtures and points per outcome

```{r, warning = FALSE}
library(dplyr) 
library(purrr) 
library(tidyr)

Home &lt;- c('DundeeUtd','Dunfermline','Inverness','Morton','StMirren') status &lt;- 'home' game &lt;- seq(1:5)

homedf &lt;- tibble::data\_frame( team = Home, status = status, game = game)

Away &lt;- c('Falkirk','Brechin','Dumbarton','QOTS','Livingston') status &lt;- 'away'

awaydf &lt;- tibble::data\_frame( team = Away, status = status,game = game)

Results &lt;- c('lose','draw','win') Points &lt;- c(0L,1L,3L) status &lt;- c('Home','Away') game &lt;- seq(1:5)

fixtures &lt;- tibble::data\_frame(home = Home, away = Away, game) lookup &lt;- tibble::data\_frame(results = Results, points = Points) rm(list = c("Away","Home","Points","game"))

```

``` {r}
teams <- c('Brechin','Dumbarton','DundeeUtd','Dunfermline','Falkirk','Inverness','Livingston','Morton','QOTS','StMirren')
pts <- c(4L,30L,52L,52L,38L,44L,58L,47L,43L,70L)

placings <- tibble::data_frame(team = teams, pts = pts) %>% arrange(desc(pts)) %>% 
  mutate(teamID = row_number(team))

team <- placings %>% select(team,teamID)
```

``` {r}
fixdf <- bind_rows(homedf,awaydf) %>% left_join(team,by = "team")

rm(list =c("homedf","awaydf"))

```

Lets try generating results for the first match.

``` {r}
first <- head(fixtures,1)

permutations <- expand.grid(first$home,first$away,status,Results)
permutations
```

create a function that will generate a nicely named table

``` {r}
get_combo <- function(x,y){
  permutations <- expand.grid(x,y, status,Results) %>% 
    rename(home = Var1, away = Var2,status = Var3, results = Var4) %>% 
    mutate(home = as.character(home),
           away = as.character(away),
           results = as.character(results))
  return(permutations)
} 
```

try for first set of fixtures:

`{r} tester <- get_combo(first$home,first$away) %>%    left_join(lookup,by = "results") tester`

now try for all the matches

``` {r}
permutations <- map2(fixtures$home,fixtures$away, get_combo)

permutations <-  bind_rows(permutations) %>% 
  left_join(lookup, by = "results")

permutations
```

``` {r}
permhome <- filter(permutations, status == "Home") %>% 
  select(home, status,results,points) %>% 
  rename(team = home)

permaway <- filter(permutations, status == "Away") %>% 
  select(away, status,results,points) %>% 
  rename(team = away)

results_df <- bind_rows(permhome,permaway)

potential <- left_join(results_df,placings, by = "team") %>% 
  mutate(poss_points = points + pts) %>% 
  arrange(desc(poss_points),desc(pts)) %>% 
  mutate(placing = rev(ntile(poss_points,10)))

potential
```

``` {r}
potential_summary <- select(potential,team,poss_points,placing) %>% 
  rename(points = poss_points) %>% 
  arrange(desc(points),desc(placing),team)
```

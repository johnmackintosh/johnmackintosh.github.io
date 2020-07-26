---
layout: post
published: true
title: 'It''s a dirty job, but someone''s got to do it..'
subtitle: A tidytext analysis of Faith No More lyrics
date: '2017-10-29'
gh-repo: johnmackintosh/FNM
image: /img/2017-10-22-AD.jpg
tags:
  - rstats
  - music
  - tidytext
---
## Is this a midlife crisis?

I wanted to ease myself back into text mining, specifically using the [tidytext package]( https://CRAN.R-project.org/package=tidytext ) as I haven't had to do any at work for well over a year. 

I've been thinking about some of the old bands of the 90's, some of whom split up, and then reformed. I was interested to see how lyrics evolve over time, and to see what changes there were as the band matured. 

![faith_no_more_epic](https://user-images.githubusercontent.com/3278367/32996943-5ed51f1a-cd81-11e7-9754-3bab31d92a0b.gif)  

I've decided to look at Faith No More, because:  
- they recently reformed after an 18 year split  
- they had numerous line up changes, the main one being a change of vocalist shortly before the release of their 3rd (and most successful) album, "The Real Thing".  
- Said vocalist, Mike Patton, was only 21 when he joined. I thought it might be interesting to see how his lyrics evolved over time  
- Their 4th album "Angel Dust" has been named the most influential rock album of all time. 

Let's get one thing out of the way - there will be some review of the band's history as well as being an R article but I promise - there will be **no wordclouds**. 

To start - let's look at the number of tracks being analysed by album. I've excluded "We Care A Lot" from "Introduce Yourself", as it's the same as the title track from their first album, and would skew the analysis. I've also excluded cover versions and bonus tracks  and a track from Sol Invictus, because  the title is **a very naughty word indeed** .

![2017-10-22-Tracks-Analysed-by-Album.png]({{site.baseurl}}/img/2017-10-22-Tracks-Analysed-by-Album.png)

Most of the examples here follow the code available on the most excellent [http://tidytextmining.com/](http://tidytextmining.com/). Big thanks to the package authors, [Julia Silge](https://juliasilge.com/) and [David Robinson](http://varianceexplained.org/) for this package. I had done some text mining prior to the release of this package, but as someone used to dataframes, I soon found my progress limited to following a small number of tutorials and not knowing how to manipulate term document matrices etc to conduct further analysis.
Tidytext is a game-changer because the data remains in dataframe/tibble format, and we can use dplyr and ggplot2 to wrangle and visualise. 

I had originally hoped this whole exercise would give me some experience with web-scraping - but the site I chose only allowed the use of an API - and while there was a Python one, there was no R alternative. Regardless - I was able to collate the data I needed. 

The original dataframe consisted of Title, Album and a lyrics column, and this was transformed into a format suitable for text analysis:  
```r
text_df <- data %>% 
  unnest_tokens(word,lyrics) %>% 
  filter(word %notin% c("b","e","a","g","r","s","i","v","la")) %>% 
  mutate(linenumber = row_number())
  ```
One song ("Be Aggressive") has a cheerleader style chorus where the title is spelled out letter by letter - so, I excluded these, along with "la", which was also featuring highly in my initial plot of common words. Along with filtering out common "stop words", I arrived at the following plot:  

![2017-10-22-Most-Common-Words.png]({{site.baseurl}}/img/2017-10-22-Most-Common-Words.png)

This looks pretty boring, so let's  at least split it by album:

![2017-10-22-Most-Common-Words-by-Album.png]({{site.baseurl}}/img/2017-10-22-Most-Common-Words-by-Album.png)  

 
### We Care A Lot  


 
![2017-10-22-WCAL.jpg]({{site.baseurl}}/img/2017-10-22-WCAL.jpg) 

![2017-10-22-WCAL-lineup.jpg]({{site.baseurl}}/img/2017-10-22-WCAL-lineup.jpg)


 Unsurprisingly, ["We Care A Lot"](https://www.youtube.com/watch?v=LQhX8PbNUWI) dominates the first album, given that it is repeated endlessly throughout the title track. Live favourite "As The Worm Turns" doesn't feature though.

### Introduce Yourself  

![2017-10-22-IY.jpg]({{site.baseurl}}/img/2017-10-22-IY.jpg)  

![2017-10-22-IY-lineup.jpg]({{site.baseurl}}/img/2017-10-22-IY-lineup.jpg)

Looking at "Introduce Yourself", I'm surprised the title track doesn't feature more heavily . Of the top words shown here, I recognise a couple from "Anne's Song". None from my under the radar favourites "Faster Disco" and "Chinese Arithmetic" though.  

### The Real Thing  

![2017-10-22-TRT.jpg]({{site.baseurl}}/img/2017-10-22-TRT.jpg)  

![2017-10-22-TRT-lineup.jpg]({{site.baseurl}}/img/2017-10-22-TRT-lineup.jpg)


Now on to the golden age of the band - Mike Patton unleashed his 6 octave vocal range and guitarist Jim Martin was still alternately shredding and providing short, clever solos. It took me a while figure out why "yeah" was so high, then I remembered the chorus of ["Epic"](https://www.youtube.com/watch?v=ZG_k5CSYKhg). Never heard Faith No More? Start with this one. Also featuring here are words from "Underwater Love", "Surprise! You're Dead!" and personal favourite ["From Out of Nowhere"](https://www.youtube.com/watch?v=j4tsGWdarHc).


### Angel Dust

![2017-10-22-AD.jpg]({{site.baseurl}}/img/2017-10-22-AD.jpg)  

![2017-10-22-AD-lineup.jpg]({{site.baseurl}}/img/2017-10-22-AD-lineup.jpg) 


"Angel Dust" is dominated from words in just 2 songs - the aforementioned "Be Aggressive" and ["Midlife Crisis".](https://www.youtube.com/watch?v=U8b88US-6ts). Other popular tracks from this album were ["A Small Victory"](https://www.youtube.com/watch?v=i9_hCjcFNO0), and their hit cover of the Commodore's ["Easy"](https://www.youtube.com/watch?v=vPzDTfIb0DU). 

### King For A Day...Fool For A Lifetime

![2017-10-22-KFAD.jpg]({{site.baseurl}}/img/2017-10-22-KFAD.jpg)

![2017-10-22-KFAD-lineup.jpg]({{site.baseurl}}/img/2017-10-22-KFAD-lineup.jpg)

"King For A Day...Fool For A Lifetime"  marks the start of the post-Jim Martin era and stands out in this plot in that it has a number of different tracks with frequently mentioned words. This album saw the band alternate many musical styles - sometimes in the same track - an example being ["Just A Man"](https://www.youtube.com/watch?v=4VcL9aMt9PQ). This was one of the album's highlights (see also - ["Evidence"](https://www.youtube.com/watch?v=7lvMNLhJrb0)) but there was some filler on there too unfortunately. 


### Album Of The Year

![2017-10-22-AOTY.jpg]({{site.baseurl}}/img/2017-10-22-AOTY.jpg)  

![2017-10-22-AOTY-lineup.jpg]({{site.baseurl}}/img/2017-10-22-AOTY-lineup.jpg)


The ironically titled "Album of the Year" reverts to being dominated by a few tracks, including single ["Last Cup of Sorrow"](https://www.youtube.com/watch?v=FtIwfugF1zw).  Other highlights were "Ashes To Ashes" and ["Stripsearch"](https://www.youtube.com/watch?v=-_U6165DVeM).

Shortly after the release of this album, and years of in-fighting the band called it a day. They didn't match the success of "The Real Thing" in the US, although the rest of the world were more switched on and "Angel Dust" actually outsold it in some areas. 

Even towards the end, they were popular enough in the UK to appear on "TFI Friday" (popular 90's live entertainment show, hosted by then top presenter Chris Evans) 2 weeks running. While Britpop was a thing, FNM played "Ashes to Ashes" and turned in one of the [finest live performances on UK TV](https://www.youtube.com/watch?v=-BhXeGemai0) (according to the internet). 


### Sol Invictus  

![2017-10-22-SI.jpg]({{site.baseurl}}/img/2017-10-22-SI.jpg)  

![2017-10-22-SI-lineup.jpg]({{site.baseurl}}/img/2017-10-22-SI-lineup.jpg)  


And, for a long time (18 years) it looked like that was that. An avant-garde rock band consigned to history. But, following several years of occasional live shows, the band returned in their final lineup and released "Sol Invictus". I have to confess I have not listened to this album, apart from a few tracks. ["Sunny Side Up"](https://www.youtube.com/watch?v=uPlmhJG_xUM) being the stand-out for me so far.

Enough history, let's look at some more plots and R code.

```r
song_words <- data %>%
  unnest_tokens(word, lyrics) %>%
  count(Title, word, sort = TRUE) %>%
  filter(n>30) %>% 
  ungroup()

ggplot(song_words,aes(reorder(word, n),n, fill=Title)) +
  geom_col()+
  xlab(NULL) +
  ylab(NULL)+
  coord_flip()+
  theme_ipsum()+
  scale_fill_ipsum()+
  #scale_fill_viridis(option ="C", discrete = TRUE)+
  ggtitle("Most Common Words - All Faith No More Studio Songs",
          subtitle = "Frequency >30. Including common stop words")+
  theme(legend.position = "bottom")
  ```
  
  ![2017-10-22-MostCommonWordsbySongIncludingStopWords.png]({{site.baseurl}}/img/2017-10-22-MostCommonWordsbySongIncludingStopWords.png)  
  
  
  ```r
  ## now remove stop words and the letters from
# "Be Aggressive"

song_words_filtered <-  data %>%
  unnest_tokens(word, lyrics) %>%
  anti_join(stop_words) %>% 
  filter(stringr::str_detect(word,"[a-z`]$"),
         !word %in% stop_words$word) %>% 
  filter(word %notin% c("b","e","a","g","r","s","i","v","la")) %>% 
  count(Title, word, sort = TRUE) %>%
  filter(n>20) %>% 
  ungroup()
  
ggplot(song_words_filtered,aes(reorder(word, n),n, fill=Title)) +
  geom_col()+
  xlab(NULL) +
  ylab(NULL)+
  coord_flip()+
  theme_ipsum()+
  scale_fill_ipsum()+
  ggtitle("Most Common Words - All Faith No More Studio Songs",
          subtitle = "Frequency >30. Excluding common stop words")+
  theme(legend.position = "bottom") 
  ```
  
  ![2017-10-22-MostCommonWordsbySongExcludingStopWords.png]({{site.baseurl}}/img/2017-10-22-MostCommonWordsbySongExcludingStopWords.png)


Term document frequency by album - the number of times a word is used in the lyrics / by the total number of words in the lyrics. A lot of words occur rarely and a very few occurring frequently:

![]({{site.baseurl}}/img/2017-10-22-Term-Frequency-by-Album.png)


Inverse Term Document frequency - or an attempt to identify words that don't occur frequently but are important.

![2017-10-22-Inverse-Term-Document-Frequency-by-Album.png]({{site.baseurl}}/img/2017-10-22-Inverse-Term-Document-Frequency-by-Album.png)

Now we've looked at common and important words, let's see if we can get a handle on the sentiment of those words. 
Admittedly, this first plot is a bit of a gimmick, but rather than use geom_col, or geom_point, I thought I'd use the [ggimage]( https://CRAN.R-project.org/package=ggimage) package, and mark the overall album sentiment using the relevant album covers. 

Here's the code:  
```r
#overall album sentiment  
albumsentiment<- text_df %>%
  inner_join(get_sentiments("bing")) %>%
  count(Album, sentiment) %>%
  spread(sentiment, n, fill = 0) %>%
  mutate(sentiment = positive - negative)

#create vector of images
albumsentiment$img <- c("2017-10-22-WCAL.jpg","2017-10-22-IY.jpg",
                        "2017-10-22-TRT.jpg","2017-10-22-AD.jpg","2017-10-22-KFAD.jpg",
                        "2017-10-22-AOTY.jpg","2017-10-22-SI.jpg")



ggplot(albumsentiment, aes(Album, sentiment, fill = Album)) +
  #geom_col(show.legend = FALSE) +
  theme_ipsum()+
  ggExtra::removeGrid()+
  coord_flip()+
  geom_image(aes(image=img), size=.15)+
  ggtitle("Overall Sentiment Score by Album",
          subtitle = "Faith No More Studio Albums - Excludes cover versions")+
  labs(x = NULL, y = NULL)+
  geom_text(aes(label=sentiment), hjust=0, nudge_y=7)+
  theme(axis.text.x=element_blank(),
        axis.ticks.x=element_blank())
 ```
        

![2017-10-22-Sentiment-by-Album.png]({{site.baseurl}}/img/2017-10-22-Sentiment-by-Album.png)

So only "Introduce Yourself" managed to get a positive overall sentiment score. I'm surprised that "Album of the Year" scores as highly as it does given the band were on the verge of splitting up. However - this is simply a case of summing positive & negative scores by individual word and finding the overall difference - so its definitely not an exact science.  Songs like "Collision" and "Helpless" do not convey positive emotion to me. 

Here is a more straightforward plot of sentiment by track:

![2017-10-22-Sentiment-by-track2.png]({{site.baseurl}}/img/2017-10-22-Sentiment-by-track2.png)

### Topic Models

![2017-10-22-Topic_Models.png]({{site.baseurl}}/img/2017-10-22-Topic_Models.png)

I'm going to take the easy way out and refer you to the tidytext website to explain topic models. I have to be honest, I don't really see any themes that make any sense to me here.  So, I thought I should look at lyrics by individual vocalist:

![2017-10-22-Topic_Models_Chuck.png]({{site.baseurl}}/img/2017-10-22-Topic_Models_Chuck.png)

![2017-10-22-Topic_Models_patton.png]({{site.baseurl}}/img/2017-10-22-Topic_Models_patton.png)

Here I take the first 5 topics by both to see if there is anything in common:

![2017-10-22-Top_Topic_Model_by_Vocalist.png]({{site.baseurl}}/img/2017-10-22-Top_Topic_Model_by_Vocalist.png)

After a bit of digging around, it turned out only 2 terms existed in both sets:

![2017-10-22-Common_Terms_between_Vocalists.png]({{site.baseurl}}/img/2017-10-22-Common_Terms_between_Vocalists.png)

Hmm,"Love" and "World". Seems a bit "touchy feely" as they say. Perhaps I should look at this by album and see what topics appear?  

![2017-10-22-Topic_Models-WCAL.png]({{site.baseurl}}/img/2017-10-22-Topic_Models-WCAL.png)  

![2017-10-22-Topic_Models-IY.png]({{site.baseurl}}/img/2017-10-22-Topic_Models-IY.png)  

![2017-10-22-Topic_Models-TRT.png]({{site.baseurl}}/img/2017-10-22-Topic_Models-TRT.png)  

![2017-10-22-Topic_Models-AD.png]({{site.baseurl}}/img/2017-10-22-Topic_Models-AD.png)  

![2017-10-22-Topic_Models-KFAD.png]({{site.baseurl}}/img/2017-10-22-Topic_Models-KFAD.png)  

![2017-10-22-Topic_Models-AOTY.png]({{site.baseurl}}/img/2017-10-22-Topic_Models-AOTY.png)  

![2017-10-22-Topic_Models-SI.png]({{site.baseurl}}/img/2017-10-22-Topic_Models-SI.png)

I'd expected perhaps that words belonging to the same song would get grouped together, but that doesn't appear to  happen very often. 

I also looked at splitting the lyrics into trigrams (sequences of 3 words, e.g. "We Care A", "Care A Lot", "A Lot We", "Lot We Care" etc. By far the most common trigram was "Separation Anxiety Hey" from "Separation Anxiety".  

I also looked at words where "Not" was the first of the 3 words in the trigram, to see if they were of positive or negative sentiment. First -the second word of the 3, then the final word of the 3. 

![2017-10-22-Negating-words-2-of-3.png]({{site.baseurl}}/img/2017-10-22-Negating-words-2-of-3.png) 

![2017-10-22-Negating-words-3-of-3.png]({{site.baseurl}}/img/2017-10-22-Negating-words-3-of-3.png)

This can be expanded to look at other types of negating words:

![2017-10-22-Negative-trigrams-Word-2-of-3-by-negating-word.png]({{site.baseurl}}/img/2017-10-22-Negative-trigrams-Word-2-of-3-by-negating-word.png)

![2017-10-22-Negative-trigrams-Word-3-of-3-by-negating-word.png]({{site.baseurl}}/img/2017-10-22-Negative-trigrams-Word-3-of-3-by-negating-word.png)


I also tried producing a network plot of the trigrams. Bit of a mess to be honest, but here it is:

![2017-10-22-correlation-plot.png]({{site.baseurl}}/img/2017-10-22-correlation-plot.png)

Finally - a correlation plot looking at closely linked overall each album is to the others, in terms of words used:

```r
library(widyr)
album_word_cors <- album_words %>% 
  pairwise_cor(Album,word,n, sort= TRUE)

album_word_cors


library(ggraph)
library(igraph)
set.seed(1234)

album_word_cors %>%
  filter(correlation > .05) %>%
  graph_from_data_frame() %>%
  ggraph(layout = "fr") +
  geom_edge_link(aes(alpha = correlation, width = correlation)) +
  geom_node_point(size = 6, color = "lightblue") +
  geom_node_text(aes(label = name), repel = TRUE) +
  theme_void()
ggsave("2017-10-22-album-word-correlations.png",width=8, height =5)

```


![2017-10-22-album-word-correlations.png]({{site.baseurl}}/img/2017-10-22-album-word-correlations.png)

This is more interesting -although - the correlations are pretty weak overall. However, The Real Thing, Angel Dust and King For A Day have the strongest correlations with each other. 
There appears to be less correlation over time between the lyrics used in The Real Thing compared to Album of the Year. 

There appears to little (or no) correlation between Album of the Year and Sol Invictus - that 18 year gap really made a difference. But what is interesting is the thick line between Sol Invictus and Introduce Yourself - 28 years apart, and from 2 different vocalists. 

However, the sting in the tail is that Mike Patton has said that he chose words more for their sound, and to fit the music, than for their actual meaning. That would explain the lyrics of " Midlife Crisis" and "A Small Victory", but seems to short change the cleverness of the lyrics in tracks like "Everything's Ruined" and "Kindergarten". Maybe this means this has been a massive waste of time? Regardless, I have enjoyed looking back at some great music from this wonderfully unique band. 

Code for the plots, and the plots themselves is available [here](https://github.com/johnmackintosh/FNM)

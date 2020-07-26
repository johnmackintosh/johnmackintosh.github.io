---
layout: post
published: true
title: Hardwired..for tidy text
tags:
  - rstats
  - tidytext
  - ggplot2
  - music
  - geniusR
date: '2018-01-30'
share-img:  johnmackintosh.github.io/img/2018-01-28-Sentiment-by-Album.png 
---
## Song lyric and sentiment analysis for all

So - a while back I did a [tidy text analysis on Faith No More lyrics](https://www.johnmackintosh.com/2017-10-29-It's-a-dirty-job/). I had thought about doing this with Metallica album lyrics, as they have had a long career, spanning their late teens/twenties to their 50's. 

However, I found the process of obtaining lyrics and getting them into shape for analysis too painful, so I chose a band with slightly less output. 

Good news though - things have changed with the release of the [geniusr](https://github.com/josiahparry/geniusR) package from [Josiah Parry](https://medium.com/@JosiahParry) (@JosiahParry).

This makes getting song / album lyrics a piece of cake. 

With my FNM analysis, I obtained individual tracks, organised them into folders by album, and then went through a lot of manual processing ( the site I obtained the lyrics from concatenated each line into a single string). 

This package does away with all that. 

Want to get all the lyrics for an album? Use the genius_album function

```r
MOP <- genius_album(artist = "Metallica",
                          album = "master of puppets", 
                          nested = FALSE)

``` 

This returns a 4 column dataframe with the tracknumber, title and lyrics. Each line of the song lyric is one one row of the dataframe, in the perfect format for manipulating further with the tidytext package.

(You can of course just obtain lyrics for individual tracks, using the genius_lyrics function).


I want to say thanks to Josiah for being super-helpful in sorting out a glitch I chanced upon, and for being patient with me when I forgot to reinstall after he fixed it!

Anyway - I soon had 10 albums worth of lyrics. Most of the code is very similar to what I did for my FNM analysis, so I don't want to get into that too much. 
I know that tidytext has been updated recently and I may go back and do some further work on this, but for now, I'm just going to stick up a few images and some notes.

Some of my earlier draft pics used the viridis palette, but as much as I love that (especially "Plasma"), it didn't look right considering I was dealing with the guys who went for black text on a black cover for their 5th and most career changing album ("Metallica").

Josiah pointed me in the direction of the [ggthemr](https://github.com/cttobin/ggthemr) package. 
Armed with that, and some online colour palette sites, I cooked up a few custom palettes based on the band's 10 studio album covers. 

Where possible, I used the relevant palette, and I also created a "Combo" palettte for use where I needed more colours:

```r  
colours_Reload <- c("#080606","#484848","#9d9c9c","#f0c43e","#fba104", "#e43609","#AD0215")

colours_Hardwired <- c("#C8C8C8","#61a28b","#e7cd9b","#7E4611","#a93119", "#2c2625","#000000")

Combo <- define_palette( swatch = c(colours_Reload,colours_Hardwired),
                         gradient = c(lower = colours_Reload[1L], upper = colours_Hardwired[7L])) 
```  

So - following the pattern of my FNM analysis, here are a few plots & thoughts:


## A brief history of Metallica

![2018-01-28-metallica-band-early.jpg]({{site.baseurl}}/img/2018-01-28-metallica-band-early.jpg)

A Danish junior pro tennis player gets into heavy metal, follows Motorhead around the UK, ends up moving to the US and places an add to start a band. In the same week, a self-confessed loner, who happens to play guitar, also places an add to start a band. They hook up. The Danish kid is Lars Ulrich, drummer and mastermind behind the band. The loner is James Hetfield, who it turns out is a Riff God in waiting. 

There have been others involved (notably Kirk Hammett on lead guitar and Cliff Burton (RIP), Jason Newstead and now Rob Trujillo on bass ) but Lars & James are the main 2, involved in all the songwriting.


### Albums - the classic first 4


- Kill 'Em All : The album that kickstarted a new musical movement - thrash metal was born in San Francisco. 
- Ride The Lightning : 1 year later, they kick things up a notch, hitting on a classic formula. Fast opener. Title Track. Power Ballad at the end of side one. Instrumental. Fast closer. Some of these tunes are still getting played night after night.
- Master of Puppets :  Another classic. The acoustic intro to "Battery" lulls the unwary listener into a false sense of tranquility.  I like "Leper Messiah" and "Orion" the most, but I'm awkward.
- And Justice For All : A bit left field. Ferocious opening and closing tracks, and also "One". Drums on 11. Bass on 1.

### The 90's rock mega-stars


- Metallica : Enter Sandman, Sad But True, Nothing Else Matters - someone you know owns this album.
- Load :  Woah, they're wearing make-up. Still, Ain't My Bitch, Until It Sleeps, Outlaw Torn, Bleeding Me and Wasting My Hate are all great tracks 
- ReLoad :  A bunch of stuff that didn't make it on to Load. Still, "Fuel" is catchy as.

###  The difficult 7th album

- St.Anger :  Wooly hats, rage, and a dodgy drum sound. And [that documentary](http://www.imdb.com/title/tt0387412/).  

### Back on track

- Death Magnetic :  Rick Rubin on production duty so it should have sounded great. It did't.  However "All Nightmare Long" is awesome and the video is pretty damn freaky.  My overall impression is that there are some great riffs, but not necessarily great songs. 

- Hardwired to Self Destruct :  A return to form. Some great tracks ("Here Comes Revenge" my favourite). "Now That We're Dead" is "Enter Sandman"-esque. The riff at 3:10 in "Atlas Rise" is guaranteed to have you tapping your feet / air guitaring.  During "Halo on Fire" they turn into The Cult. This is a Good Thing. These are all Good Things

![2018-01-28-Metallica-band.jpg]({{site.baseurl}}/img/2018-01-28-Metallica-band.jpg)


OK -  all ready? Let's "Hit The Lights": 

![2018-01-28-Most-Common-Words.png]({{site.baseurl}}/img/2018-01-28-Most-Common-Words.png)

I've only heard 2 tracks from St.Anger (lucky me). Produced at their lowest point, with an "acquired taste" drum sound ( Lars Ulrich forgot to switch the snares on for one recording, and liked the sound, so kept it for the entire album) and alongside the title track , "Frantic" is the other. It's got a pretty repetitive chorus, which is why "Tick" is the most common word overall, though I did get a surprise when I first saw this. 
Altogether now - " tick tick tick tick tick tick tick tock"
No? 
It's actually annoyingly catchy.

![2018-01-28-Most-Common-Words-by-Song-Excluding-Stop-Words.png]({{site.baseurl}}/img/2018-01-28-Most-Common-Words-by-Song-Excluding-Stop-Words.png)


### Can I  get a "Yeah"?


Let's dig in to this at album level:

![2018-01-28-Most-Common-Words-by-Album.png]({{site.baseurl}}/img/2018-01-28-Most-Common-Words-by-Album.png)

The first thing that I want to highlight here is I couldn't for the life of me get the bars to go into the correct descending order, despite copying the code precisely as I have used it several times before. 
In the end, I had to publish and be damned, but - I KNOW! - it's not 100% correct. 

The other  point to note is that "Yeah" features heavily throughout the 90's albums (Metallica, Load & Reload).  I pretty much stopped listening to the band sometime between "Metallica" and "Load" - and have to confess hadn't picked up on this. (I did buy "Load" years later out of curiosity (and it was on sale!). It's a good album. There's some good stuff on there). 


Term Document Frequency - finding most commonly used words, and the inverse term document frequency, finding words that are rarely used but also important:

![2018-01-28-Term-Frequency-by-Album.png]({{site.baseurl}}/img/2018-01-28-Term-Frequency-by-Album.png)

![2018-01-28-Inverse-Term-Document-Frequency-by-Album.png]({{site.baseurl}}/img/2018-01-28-Inverse-Term-Document-Frequency-by-Album.png)


Looking at the sentiment of the band's output, and guess what? They're not exactly shiny happy people,at least, not on paper:


![2018-01-28-Sentiment-by-Album-timeline.png]({{site.baseurl}}/img/2018-01-28-Sentiment-by-Album-timeline.png)

I produced a couple of plots for the sentiment by track:

![2018-01-28-Sentiment-by-track.png]({{site.baseurl}}/img/2018-01-28-Sentiment-by-track.png)

The more traditional bar plot, and also this one using geom_point instead:

![2018-01-28-Sentiment-by-track-point.png]({{site.baseurl}}/img/2018-01-28-Sentiment-by-track-point.png)

I quite like this one to be honest, although I could do with a bigger monitor to fit everything in.

I also produced more of a timeline strip plot by release date. 

Here's the positive sentiment score by track over time:

![2018-01-29-Positive-Sentiment-by-track-timeline.jpg]({{site.baseurl}}/img/2018-01-29-Positive-Sentiment-by-track-timeline.jpg)


Judging by this - "Master of Puppets" has the most positive net sentiment.


"St.Anger" is the most negative, followed by "All Within My Hands". (For this plot, I multiplied the negative sentiment by -1 to force it below the axis):

![2018-01-29-Negative-Sentiment-by-track-timeline.jpg]({{site.baseurl}}/img/2018-01-29-Negative-Sentiment-by-track-timeline.jpg)

"St.Anger" also scores highly for positive sentiment, leaving  "All Within My Hands" as the most miserable net sentiment score.

![2018-01-29-Sentiment-by-track-timeline.jpg]({{site.baseurl}}/img/2018-01-29-Sentiment-by-track-timeline.jpg)



### Topic Models

As with last time, I’m going to take the easy way out and refer you to the tidytext website to explain topic models. Here they are for each album, with custom , cover specific colours. 

![2018-01-28-Topic_Models-KEA.png]({{site.baseurl}}/img/2018-01-28-Topic_Models-KEA.png)


![2018-01-28-Topic_Models-RTL.png]({{site.baseurl}}/img/2018-01-28-Topic_Models-RTL.png)


![2018-01-28-Topic_Models-MOP.png]({{site.baseurl}}/img/2018-01-28-Topic_Models-MOP.png)


![2018-01-28-Topic_Models-AJFA.png]({{site.baseurl}}/img/2018-01-28-Topic_Models-AJFA.png)


![2018-01-28-Topic_Models-Black.png]({{site.baseurl}}/img/2018-01-28-Topic_Models-Black.png)



That's right, I found several shades of black for you. 


![2018-01-28-Topic_Models-Load.png]({{site.baseurl}}/img/2018-01-28-Topic_Models-Load.png)


![2018-01-28-Topic_Models-Reload.png]({{site.baseurl}}/img/2018-01-28-Topic_Models-Reload.png)


![2018-01-28-Topic_Models-Anger.png]({{site.baseurl}}/img/2018-01-28-Topic_Models-Anger.png)


![2018-01-28-Topic_Models-Magnetic.png]({{site.baseurl}}/img/2018-01-28-Topic_Models-Magnetic.png)


![2018-01-28-Topic_Models-Hardwired.png]({{site.baseurl}}/img/2018-01-28-Topic_Models-Hardwired.png)



Almost done... 

Some words have "not" or other negating words immediately in front of them, so this can change the meaning / sentiment. Which words does this most commonly affect?


![2018-01-28-Negative-trigrams-Word-2-of-3-by-negating-word.png]({{site.baseurl}}/img/2018-01-28-Negative-trigrams-Word-2-of-3-by-negating-word.png)


![2018-01-28-Negative-trigrams-Word-3-of-3-by-negating-word.png]({{site.baseurl}}/img/2018-01-28-Negative-trigrams-Word-3-of-3-by-negating-word.png)


I looked at correlations of individual words, but it was not very informative, however, looking at the words used by album, there was quite a nice pattern emerging:

![2018-01-28-album-word-correlations-original.png]({{site.baseurl}}/img/2018-01-28-album-word-correlations-original.png)


Although the correlations are weak - we can see Metallica, Load and Reload being very closely linked. At the other end, Death Magnetic, Hardwired and Kill 'Em All are fairly closely linked (30 + years apart) and in the middle are the classics - Ride The Lightning, Master of Puppets, and also And Justice For All. 

I'd expected there to be a clear difference between the newer stuff and the old stuff. I certainly didn't expect to see "Kill 'Em All" and "Hardwired.." so close together. However-  if you watch the "making of" videos on YouTube, you can see the bands writes the guitar and drums first, and then James adds lyrics later - and for the latest album, the rest of the band were also pitching in. I suspect that the lyrics aren't perhaps as important as the music - certainly not compared to the And Justice For All period where everything seemed to tie together more.

To wrap up, this has been a much smoother task second time round, (I wasn't planning to do this again) and if I ever do it again, going to have to look into purrr to make things even easier.

Thanks to Josiah - cool guy, cool R package!

---
layout: post
title: "rockthemes"
subtitle: Classic rock album covers inspire even more R colour palettes
share-img: /assets/img/logo.png
published: TRUE
tags:
- rstats
- music
comments: yes
---


If you'd have asked me a few weeks ago if I wanted to create an R colour palette package, I'd have said "No way".

In fact, I'd have added 
"There are already **MANY** R colour palette packages, some might even say _too many_.
The last thing the world needs right now is more R colour palette packages"

Imagine my surprise when I made [metallicaRt](https://johnmackintosh.com/metallicaRt/), even if I did royally mess up the announcement in my last blog post, by originally failing to include working images.

Still, got there in the end, and I was safe in the knowledge that at least I wouldn't be doing *that* again.

Despite the fact that {metallicaRt} bombed spectacularly on  R-Bloggers (its OK, I'm not taking it personally^), it did get shared on LinkedIn, of all places, and seemed to be very popular. So much so, that I felt I should go back and make it a bit more ggplot2 friendly. 
Which it is -  you can now [scale_fill_reload](https://johnmackintosh.com/metallicaRt/reference/reload_pal.html) or [scale_colour_anger](https://johnmackintosh.com/metallicaRt/reference/anger_pal.html) to your heart's content.  
Go on, [take another look](https://johnmackintosh.com/metallicaRt/index.html).


But that is not all. 

Metallica are great, but there are other bands out there (tell that to my playlist).
And some of them have visually appealing album covers, that go beyond black and red as the main colourways.

And so, as much to my astonishment as it will be to your general disinterest, I present: 

[rockthemes](https://johnmackintosh.com/rockthemes/)

I'm actually quite pleased with this.

First things first, it's not _all_ rock and metal bands 

Some are included because they _are_ rock classics. 

Others made the cut because I like the band, even though they may have done better albums. 

Some are included because the covers are colourful (or weird) and others are in simply because they came up when I searched for a list of classic rock album covers.

I will leave it to you to check them out. 
In amongst them all, you will find themes inspired by acts such as Taylor Swift, Oasis, The Cult, Uria Heep (not a typo),  PJ Harvey, Janelle Monae, The Go Gos, Nirvana and  No Doubt. 

The latter came in via PR, before I'd even got my website properly set up.
And, the resulting palettes were so good, they immediately became my favourite and made it onto the package logo. 
And because the logo palette was so good, I'm only _slightly_ annoyed about the fact _I_ didn't come up with it in the first place. 


Please go and check this package out, and if you like it, star it. 
If you use it,  I'd love to see it in action.  (I already have an example of {metallicaRt} being used for a football heatmap).


Of course, it wasn't all just mucking about looking at album pictures and making palettes. 
I also managed to use github actions to run package checks, which I've done before, but what I really wanted was to automate the rendering of the readme, and the building of the package site. 

It's fair to say pkgdown site building and missing images drove me fairly demented over the last night or two, but I think it's all working now. 
For the life of me, I could not figure out why my images were not dispaying on my package site. 
I was knee deep in github issues, trying to find an answer. 

The images were displaying as missing links in Chrome. 
But, on right clicking, there was an option to load the image, and when I did *that*, they appeared. 
And then I remembered that .jpg files often get blocked, while .png don't. 
So I converted them all to .png and my website displayed correctly as intended.

TWO NIGHTS OF getting nowhere, solved just like that!


I'm a big fan of github actions once they are up and running.

I will blog more about this, hopefully, but in the meantime, please check out the veritable cornucopia of visual delights^^ over on [rockthemes](https://johnmackintosh.com/rockthemes/)


^ I am  
^^ It's possible I've overdone it there. 








 

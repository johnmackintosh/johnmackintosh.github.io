---
layout: post
title: "Introducing popthemes"
share-img: /assets/img/popthemes/aqua.png
published: true
tags:
  - ggplot2
  - rstats  
subtitle: Introducing another set of palettes and scales for ggplot2, based on pop album covers.
comments: yes
---

Back once again with the block rocking themes!

Yes, having produced [a set of R colour palettes based on Metallica album covers,](https://github.com/johnmackintosh/metallicaRt) and then another set of palettes based (loosely) on [classic rock album covers](https://github.com/johnmackintosh/rockthemes), I've decided to complete the set with {popthemes}

One problem with {rockthemes} is that I got carried away, and I have way more palettes than I intended. With {popthemes}, I've kept it to a set of 12 palettes, and will probably leave it at that.

The problem of course, is that I have no real care for pop, so a bit of googling was called for.

As usual, interesting and colourful \> musical ability in terms of choosing what made the final cut.

So let's see, who made the final dozen?

![](/assets/img/popthemes/aqua.png "Aqua")

    Aqua. It's colourful.
    

Aqua? They sung a song about Barbie, and had a very colourful album cover. That's all I know.

![](/assets/img/popthemes/bangles.png "Bangles")

    The Bangles - In your room
    

The Bangles were always getting in, because their cover of 'Hazy Shade of Winter' is absolute banger. However, it was hard to find a decent cover, this was the best I could find, from their 'In Your Room' single. No idea what that was about, and I can't be bothered looking it up. However, as father of 3 boys, I can confidently claim that if I wrote a song called 'In your room', it would contain lots of complaints about Lego and an exhortation to 'sort this mess out'.

![](/assets/img/popthemes/beck.png "Beck")

    Beck
    

Beck - Midnite Vultures. This has got 4.5 / from 118 reviews on Amazon. It's also colourful.

I don't know what else to tell you.

![](/assets/img/popthemes/boo.png "Betty Boo")

    Betty Boo - Where Are You Baby?
    

1990 was a heck of a year in the UK. [Just look at this list of tracks](https://en.wikipedia.org/wiki/List_of_UK_top-ten_singles_in_1990) that made the top ten throughout that year. New Kids on the Block, Kylie, Vanilla Ice, and Jive Bunny.

In amongst all this madness, along comes Betty Boo, with an absolute [choon](https://youtu.be/H8iDrGW39EA). If you haven't heard it before, then it's a bit of an ear worm. You've been warned. I love the beat, I keep thinking this reminds me of [Cornershop's 'Brimful of Asha'](https://youtu.be/5LBnMRWeV-E) (another belter), but this came out first. I also love the way she 'na na na na's' the guitar solo. Legendary stuff. If forced, I would probably say Chris Isaak's 'Blue Hotel' was the best track of the year, based on the list that I've linked, but this would run it close.

I was going to go with 'Doing the Do', if only because I could then use the terrible pun 'Doing the Do.Call', but as you can see, I decided not to, because I'm better than that.

![](/assets/img/popthemes/bwitched.png "B*Witched")

    B*Witched is really hard to type
    

B\*Witched. Bright orange album cover. I don't really know too much about them TBH, although I believe they wore a lot of denim.

![](/assets/img/popthemes/deeelite.png "Deee-Lite")

    Deee_Lite - Groove Is In The Heart
    

Deee-Lite. This is a stone cold classic. The video [is wild though.](https://youtu.be/etviGf1uWlg)

![](/assets/img/popthemes/Hole.png "Hole - Celebrity Skin")

    Hole - Celebrity Skin
    

Hole - Celebrity Skin. A great piece of pop- punk. Is that what they were? Dunno, but this is good. I think it's the only track of theirs I've actually heard, but it's a favourite.

![](/assets/img/popthemes/nodoubt.png "No Doubt - Tragic Kingdom")

    No Doubt - Tragic Kingdom
    

No Doubt - Tragic Kingdom. This theme was submitted to {rockthemes} and it's such a great one, I've stuck it in here too (modified slightly because all these palettes were made using different tools). I love it. I ported it to PowerBI as well, so that I could sneak some \#rstats goodness into my work.

![](/assets/images/popthemes/astley.png "Rick Astley")

    rickroll
    {:.figcaption}

Rick Astley. Never Going to Give You Up. This [does what it says on the tin](https://youtu.be/dQw4w9WgXcQ)

Into the home straight now.

![](/assets/img/popthemes/sclub.png "S Club 7")

    S Club 7
    

S Club 7.

Who among us has not been on the dancefloor [to this one](https://youtu.be/vm262cXxRrU)?

I don't believe, you, because even I have, and I hate being on the dancefloor.

![](/assets/img/popthemes/spice.png "Spice Girls")

Spice Girls.

They were the biggest band on the planet. No particular track, so pick your favourite from the Spice World album.

![](/assets/img/popthemes/steps.png "Steps")

    Steps
    

Steps - This has to be the ultimate pop palette surely? Again, pick a track to suit - thought I guess for us data people it should be '5,6,7,8'?

I originally named the palette scale_fill_steps, then realised I'd overwritten someting in ggplot2. So I changed it to scale_fill_steps2. Nope, that exists as well. So it's now scale_fill_steps_h, after , well, 'H'. It's probably going to have to get changed again, at which point I'll just call it HFCL, to prove that somehow, my knowledge of this band is above and beyond what it is necessary.

You can find the [package on github](https://github.com/johnmackintosh/popthemes "popthemes"). I have no plans for CRAN, although I have discovered that some of the basic {rockthemes} palettes have made it into the paleteer package (somehow!), so , who knows?

As always, if you like it, please star it - it costs you nothing. And, if you use it, and you can share it, I'd love to see any plots you produce.

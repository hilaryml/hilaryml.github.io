---
layout: post
title:  "Gotta Catch 'Em All - My Pokedex CLI Gem"
date:   2016-07-24 01:58:28 +0000
---


After spending an hour catching Pokemon in the park with my dog this morning, coming up with an idea for my CLI gem wasn’t as stressful as I thought it would be. 

It hit me when a friend of mine told me that when I eventually manage to evolve my Magicarp (after catching 99 more), not only will it evolve into the badass Gyarados, it will also become a water-flying type Pokemon… Whaaat?

Who else was surprised/forgot that Gyarados wasn’t a dragon type?? What the heck?!

For any of you feeling the pangs of nostalgia as you run around town trying to remember the names and types of your favourite childhood Pokemon, I’ve got your back. I’ve built a program that will not only print out the original Gen 1 Pokemon by number, name, and type, but will also print out their Pokedex entries (which includes habitat - though I’m not sure just how accurate this will be for those of you using this info to play Pokemon Go, seeing as I caught a Seadra and a Horsey in the middle of a forest this morning…). 

Also, for those of you who are not aware of the childlike innocence and overall hilarity of some these pokedex entries, I raise you this:

![](http://25.media.tumblr.com/tumblr_m83b734B9m1qbumrto2_1280.jpg)

and this too 'cause it's funny:

![](http://i.imgur.com/ttB1e.png)

I decided to start by watching the provided CLI Gem Walkthrough to get focused. While watching, I used bundler to help set up the skeleton of my gem and then went about setting up the basics of my CLI file so that it reflected what I wanted my program to eventually be able to do. That is - to print out super important info about super awesome Pokemon!

I then chose which site I was going to scrape my info from. I settled on pokemon.wikia as it was the only one I could find with the type of detailed info I was looking for. Once I found the css to grab the name, number, and type of each Pokemon, I also grabbed the urls for their Pokedex entries to get the info for their additional attributes. From the Pokedex entries I got the css for their physiology, behaviour, natural abilities, and habitat. I found this part to be a bit tricky, as nothing in the body of the HTML was nested, or organized in any real way. There were no classes or id’s for any of the sections, and all of the elements on the page were all siblings with the exact same parent and the exact same tag. Rats! This meant that I ended up having to grab large sections of unnecessary text and figure out a way iterate through so that I could grab just the stuff that I actually needed. It took quite a while to figure out how to do this part, but once I did I felt like a real Pokemon MASTER.

Not all of the Pokedex info was available for each Pokemon, so I had to account for that in my code. I decided to do that with a case statement. I then made a Pokemon class to make new Pokemon objects, assign them their attributes, and then store all of the Pokemon instances and their attributes.

After that I went through all of my files and did a bunch of retouching, including reformatting code so that it looked cleaner, fixing a bunch of bugs, and just getting it to look and work efficiently. Unfortunately at this point I noticed that the program is pretty slow off the mark. It takes a solid 20 seconds to load the initial print out which sucks. After that it is speedy but I decided to look into ways of reducing the initial load time for the program. After changing a few things in my code I was at a loss. I decided to check out the speed of the actual webpages I was scraping and found they were a bit slow too, so perhaps that’s where the problem originates??? I'm still looking around online to see if there is anything else I can do, so if anyone has any ideas, feel free to check out my code on github and let me know!

In the meantime, good luck Pokemon Trainers! 


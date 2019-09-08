---
layout: post
title: "The Graphics Module"
date: 2019-09-03 10:00:00
author: openjge-dev
---
My apologies; the past four weeks have been quite busy, with work (and a bit too much No Man’s Sky) leaving me with not a lot of development time. With that being said, I managed to scrape together a very very basic rendering system packaged, which I’ve packaged up into the Graphics module. All that’s left now is to right up a fragment shader to handle lighting calculations.

There isn’t too much to talk about here without getting into the nitty gritty technical details, but completing the Graphics system really gave me a chance to test out the engine as a whole, which revealed some very urgent issues in the Core module that I’d previously overlooked. It also meant rewriting some of the underlying systems, most notably to the state updates. Prior to starting the Graphics module, updates to IState objects occurred once per loop cycle, after each component contained in it had been updated. However, because I needed to perform pre-rendering operations for each state before it or it’s component could update, I was forced to create a new method for the IState interface: updatePrep. This method would be called before component updates or the normal state update ran, and in the case of the Graphics module, included things such as binding the correct shader program.

What was undoubtedly the biggest issue worth mentioning, though, arose in the (surprise, surprise) ThreadPool class. What I’d previously though as an impenetrable fortress of code soon succumbed to a host of particularly nasty bugs which required me to rework a fair bit of the task scheduling. If you’re interested, you can check out these changes in the GitHub repo. In short, lock-free, resizable ring buffers are hard to get right in a concurrent environment (and not something I can quite pull off).

All in all, I have to admit, I’m not as happy with the Graphics module as four weeks worth of work should constitute, and this is mainly because it feels like a bit of a compromise. As I said above, the module is pretty simplistic in nature, which is by no means *bad*, but means that I wasn’t able to add some of the features I’d initially hoped to. For example, before starting to work on the module, I had been wanting to implement some sort of spatial partitioning structures such as a quadtree in addition to the scene graph. But instead of being able to fulfil this vision, I struggled to find ample time in which I could concentrate on designing and writing up these new features. That’s not to say that they’ll never get added, because I’ll try to get around to them after I finish up some other important parts of the engine, but I just feel a bit disappointed that I couldn’t have gotten it done with the rest of the Graphics module. Oh, well.

![Whoops - no image :(](https://openjge.github.io/OpenJGE2D-Website/img/posts/First-Render.jpg "The First Render!")

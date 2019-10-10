---
layout: post
title: "One Step Forward, Two Steps Back"
date: 2019-10-09 21:40:00
author: openjge-dev
---
So I’m not quite done what I’ve recently begun working on, but a new post is long overdue and I thought I should give at least a brief update on how things are going. Yes, I’m still here working away although, as predicted, school has been pretty hectic lately, leaving me with very little time to sit down and continue designing stuff. Anyway, as you probably know from the last devlog, I finally got around to “finishing” the graphics module. And while it is fully functioning (I can correctly render both luminous and non-luminous sprites), it leaves a lot to be desired, not to mention that much of the features added feel like hacks that’ll hurt the stability of the engine later down the line. For example, I had to write this convoluted State/Scene counter in the Renderer so that it knows when to clear colour and depth buffers and swap the window buffer.
I’ve also just realized that the rendering engine won’t render transparent/translucent objects properly because none of the objects in a scene are being ordered back to front. Solving this issue, though, is stifled by the very design of the graphics module, as each scene is fragmented into multiple render state objects based on shader type - not taking each object’s depth into account at all. And because components are rendered directly from the update method using the `Graphics.Module.drawModel` method, there’s no way to stop a component held in State One from being rendered until after a component located farther back in the scene but held in State Two is rendered. The only workaround is storing the depth of each component internally in the renderer, but that breaks the entire graphics module itself, leaving the current sorting mechanisms (based on opacity/translucency and shader program) redundant.
So this leads me to the feature I’ve been currently brainstorming, which is a revamped graphics system (graphics 2, if you will) that’s meant to fix most, if not all, of the issues affecting the current system. This is also why I’ve refrained from writing the documentation for said system as well as merging the `feature/Graphics` branch into the `develop` branch on the engine’s Github repo. As of now, I’m still firming up the design in my head and on paper, but hope to have some flowchart documents and a proper breakdown of what I’ll actually be doing sometime within the next week or so. Basically, I’m going to be switching to a bucketed rendering technique where draw calls for components are put into specific buckets, sorted by each component’s properties, and deferred until later. With this setup, each bucket represents a separate render pass. Unfortunately though, it’s starting to look like that I’ll need to make some slight changes to how the Core module interacts with other modules for this new system to work out, but it shouldn’t be too hard to sort out.

For those of you interested in following along, here are the resources that I’ve been reading up on recently that break down the structural rendering techniques I’m trying to incorporate:

[Ordering your graphics draw calls around!](http://realtimecollisiondetection.net/blog/?p=86)

[Stateless, layered, multi-threaded rendering](https://blog.molecular-matters.com/2014/11/06/stateless-layered-multi-threaded-rendering-part-1/) - Parts 1-3

[Command buffers for multi-threaded rendering](http://alinloghin.com/articles/command_buffer.html)

[Understanding data flow in a multi-threaded render pipeline](https://www.gamedev.net/forums/topic/695433-understanding-data-flow-in-a-multi-threaded-render-pipeline/)

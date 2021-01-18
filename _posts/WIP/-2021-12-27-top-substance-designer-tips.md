---
layout: single
title:  "Top Substance Designer Tips"
excerpt: ""
categories: 
toc: true
---
## A

There are many writings and tutorials on the internet which cover the basics or specific workflows; so what I've done here is an attempt to find some more less documented topics, and if you manage to take anything at all away from this, then success!

#10 Prevent jaggies on your material renders
Who doesn't love displacement?  It's the last layer of "phwoar" to a material render, which (at the time of writing) is a luxury we don't often get to use due to the GPU cost on widespread common hardware.

use rocket button in marmsoet to turbo when doing scene edits



#9) - Custom functions are your tidy friends that will save you time and keep things neat
There's a common expression in programming "don't repeat yourself". Ultimately, your graph is a big slate of preconstructed code blocks ready to get compiled on a per-pixel level every time an update is called. If you ever find yourself taking a handful of nodes, and only adjusting a slider or two, copying and pasting it multiple times because you have different sources, then stop! Split that chunk off into another graph, expose the parameters you need, and don't forget to include an input node at the start. 

This is a function as it takes an input, then works it's magic and returns an output. Publish that sub-graph as a SBSAR, and then reference it in to your larger graph. You won't have spaghetti nodes, your sub-graphs will be clear to as what they do, you'll get great tool usage for future projects for when you tackle a similar problem in the future.

#8) Show some love for 16-bit dithering
#7) Compare FX-Maps scatter to scatter node
#6) Bevel using the appropriate algorithm between linear and curved 
#5) Show some love for SVGs
lighter file. link the file. external tools more control. better than rasterised sources. touch on designer height strokes maybe? maybe Ai.

plug node I made for RGBA colour to mask

no stroke tool in designer yet

#4) Big, medium and small forms
stand away from monitor on other side of room. can you still read it? render with low camera fov for a more cinematic focal length.

#3) Min and Max mask your shit together
ultimately, learn your blending modes. understand how each pixel interacts rather than shotgun blend mode testing

list all blend modes and breifly describe the popualr ones. and brief use cases



histogram scan and then mask it over the top to layer up things. 

#2) Control your 0-1 height. range
 good distributiob. may have heard start with height etc

#1) FX MAPS / pixel processer power - learn to code - give wicked example


 █████╗ ███████╗██╗  ██╗██╗ ██╔══██╗██╔════╝██║ ██╔╝██║ ███████║███████╗█████╔╝ ██║ ██╔══██║╚════██║██╔═██╗ ██║ ██║  ██║███████║██║  ██╗██║ ╚═╝  ╚═╝╚══════╝╚═╝  ╚═╝╚═╝




Banding or contour lines to avoid use, BLEND IT AS A BACKGROUJD NOT AS FOREGROUND.

Split graphs apart - example of marble gold trim

Use generators as sample sources - show example

Avoid the stock grunge maps where possible

Material planning - approaching an environment

Use marmoset and keep auto export on. Pin window on top using deskpins if you have one screen

8-16 bit dithering for portfolio work. explain costs

Sharing is caring. Soon as you publish post it on twitter, discord and polycount for coverage. Touch on popularity a bit here.  
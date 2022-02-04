---
layout: single
title:  "The Dappled Forest"
excerpt: "Global Game Jam 2022 project."
categories: game
toc: false
---
## The Dappled Forest

I took part in the Global Game Jam 2022 this January, and this is the end result...

![Alt](/antskilton.github.io/assets/images/2022-02-04-the-dappled-forest/theDappledForest.gif "Moving between light and dark")

We met on the London jam Discord, and due to covid restrictions, instead of cramming everything in on the weekend, I was able to pace
out the Sunday throughout the week, which also allowed people to discuss ideas before the weekend too.

The theme was "duality", and we ended up landing the staple reliable theme of light and shadow.
Stealth games favour the shadow, whereas horror games favour the light, I wanted to concentrate
more of the equality of them. The Yin and the Yang. Dappled light through leaves has a harmony to it
so I thought 

To turn it into a game I thought a timer (1 minute on the clock) and a points system gaining points over time in either
light or shadow (as directed by the objective text) would be the core concept just to get us going. I started off by getting into Pro Builder and hacking out some shapes out.
With a directional light above the environment, they would cast perfect silhouettes down below.

![Alt](/antskilton.github.io/assets/images/2022-02-04-the-dappled-forest/theDappledForest2.gif "Shape silhouettes")

Taking those shapes, I extruded them to make volumes which would then become the collider volumes. Duplicating these volumes, we could then
make 3 arrays of light volumes, shadow volumes and those original shape masks. If we start with the light volumes hidden, we can randomly iterate over 
these arrays together toggling them randomly to show and hide chunks of the forest. Sometimes we invert everything to add a layer of complexity to make it less predictable. 
Timings between each iteration was another variable to play with. A collider on the player with some score iterating logic, with a level score target, and we have ourselves a game loop.

Next up was to get some art in, so I downloaded a forest pack I saw on Kenney's royalty free assets. Same for the UI (with a Google font). I had some fun and made a campfire,
hooked up a frontend and a brief win/lose screen. Some other members of the team made some fitting audio which really brings the forest to life.

Another team member got the royalty free character controller operational along with a health bar system, which adds a level of difficulty to the last level.
We baked a nav mesh and got some bugs scuttling around in, to finish it off. The game is [available to play for free on itch.io](https://mcmlxxxiv.itch.io/dappled-forest).
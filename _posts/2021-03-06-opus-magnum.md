---
layout: single
title:  "Opus Magnum: Game Digest"
excerpt: "Soft programming wearing the mask of an alchemist."
categories: game-review
toc: true
---
## Alchemy 101

From developer [Zachtronics](https://store.steampowered.com/developer/zachtronics/), similar to some of their other titles, [Opus Magnum](https://store.steampowered.com/app/558990/Opus_Magnum/?curator_clanid=32946839) is a soft programming game themed in a fantastical scholar based world, akin to Potter like schools set over a period megacity. The dialogue is engaging too based on a mild story as you "play" the role of the alchemist, Anataeus Vaya.

Thematics aside, the game presents to you a problem to solve, known as `products`, which can only use the `reagents` provided for a level. Using mechanics such as arms and bonding devices, you must create the products in a way you see fit. You are graded on three main areas, cost (total quantity of mechanics you use), cycles (how long the total operation takes to complete 6 products), and area (the total footprint used for the level). As you can imagine, the metagame is based around optimising a level, and considering other solutions which could be a balance of all 3 factors, be take one factor to the extreme or just be a visually beautiful machine.

![image-center](\assets\images\2021-03-06-opus-magnum\gameplayObjects.png){: .align-center}

These are the majority of tools to construct your machine with, with their costings. You place the products and reagents too.

![image-center](\assets\images\2021-03-06-opus-magnum\timeline.png){: .align-center}

The timeline is what you use to input all of your instructions to make the machine work. Each track represents a mechanical arm in the level.

## Hey, at least they work!

Here're my solutions. At the end of a level you can see a histogram to compare your solution to the community's. Fittingly, there's a gif recorder too, just like they knew!

![image-center](\assets\images\2021-03-06-opus-magnum\Curious Lipstick.gif){: .align-center}

Easy to deconstruct here, two functions operate and join to make the product. The blueprint is large, the cycles are average-long but it's cheap.

---

![image-center](\assets\images\2021-03-06-opus-magnum\Seal Solvent.gif){: .align-center}

New mechanics are introduced such as morphing elements into salts, paired here with the linear morphing of metals.

---

![image-center](\assets\images\2021-03-06-opus-magnum\Alcohol Separation.gif){: .align-center}

Once comfortable with bonding the elements together, you're then introduced to seperating them, fitting name this level.

![image-center](\assets\images\2021-03-06-opus-magnum\Litharge Separation.gif){: .align-center}

I like how compact this is, with the dedicated element seperation to each side.

---

![image-center](\assets\images\2021-03-06-opus-magnum\Alchemical Jewel.gif){: .align-center}

The bond locations become important, so attention to assembly becomes the focus of some prettier machines.

---

![image-center](\assets\images\2021-03-06-opus-magnum\Golden Thread.gif){: .align-center}

Infinite machines are essentially for loops with a pre loop void grab at the start. One criticism would be to allow the ability to loop a section of the instruction track rather than a generic "loop whole track" to simulate for loops easier. I find utilising rails a cheap and efficient way to fix offsets, particularly when space is tight.

---

![image-center](\assets\images\2021-03-06-opus-magnum\Purified Gold.gif){: .align-center}

You just knew it was coming near the end, when you'd have to make the linear path from tin all the way to gold. I chose a cycle focus here. If there were any additional elements, you'd begin to notice the expontial slowdown of gold output. Small satisfaction with the 2 to 1 element scaling with that the arms never collide.

---

![image-center](\assets\images\2021-03-06-opus-magnum\sigmarsGarden.png){: .align-center}

Intermittently throughout chapters you can have a breather with a custom implementation of solitaire named Sigmur's Garden. Visually sticking to the hex grid, cardinal elements and metals is an ideal way to theme this.

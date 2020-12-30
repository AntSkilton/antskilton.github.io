---
layout: single
title:  "Procedural Dungeon Design Part 2: 3D Pieces"
categories: procedural-generation
toc: true
---

## Starting with the Floor

The most essential piece of geometry in any character driven game is the floor of whatever form in comes in. Throughout this series, we'll be diving into TEST






















2 3D Pieces
the grid
breaking down the level into tile types
piece tolerances and boundaries
no compound pieces
make sure they work together on a geomtry level
atlases and tilables - shaders that support WS tiling
base grid level versus base art level


3 Piece Definitions & The Database
piece and placement definitions
requirement tiles
orientation for offset grid walls
the database
random variation selection
elevation
biome theming

4 Unity Pipeline
User facing 2d grid tool
scriptable objects and their types all coming together explained here
Algorithm and technical implementation (piece priority, piece mirroring)
Creation pipeline and export processes

5 Level Creation in Unity
Piece Palette
drawing tool design
gameplay elements
visual polishing and non PCG content. non tool appropriate art

- - -



Everything in this diorama concept is of course implementable, but at what cost?
(Yes.. the answer is time)

Everything outside of the orange box I would consider feasible for the tool and kit pieces to be implemented appropriately.

Content inside of the orange box here is outside of the tool’s scope as it contains elevation, enormous overhangs and a large bespoke tree, but yes it does look cool.

However, if this asset was used in a large quantity of rooms, it made this biome feel unique (maybe it was a church bell tower with environment storytelling reasons), then this asset becomes much more viable. 
We need to avoid single use cases wherever possible.

How much time would it cost to have it?

Assuming we have all of those resources (vegetation, walls windows etc), this piece will have to exist outside of the tool and be manually placed in the room prefab. It would be liable to regressions as it’s outside PCG.

Finding the balance
Ideally, environment concept designers should understand the pieces in this presentation and use that as a strength to drive the majority of their work.

When it comes to orange box related content, ask “is it so tremendously cool it adds to the environment storytelling, it needs get out there”? Ultimately, it’s up to leadership to decide with human power and timing budgets.

Can this piece be used a lot?
Does the biome feel incomplete without it?
Would something viably replace this in other biomes?
Have 3D got time to implement this in addition to everything else?

If all of the above answers are yes, then great! (but those aren’t easy requirements)
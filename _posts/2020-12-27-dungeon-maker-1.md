---
layout: single
title:  "Procedural Dungeon Design Part 1: Overview"
categories: procedural-generation
toc: true
---
## What *Exactly* is it?

Procedural generation / proc-gen or PCG for short, has been around since the late 70s is by no means a new concept to video games. It's the idea of taking a bunch of parameters, having them set none, some or all of those dials to something which may influence the end result. These modifiers are applied to an input model, which also gets passed through an algorithm in order to get a predetermined(ish) result which hopefully the end user would think "hey that's pretty neat".

Determinsitic is an important key word here, which is to be favoured over "random" due to it's quasi calculated end result which can be achieved (pending any random seeds); whereas as true randomness is assumed to have no logically sound reproductive steps. In summary we could refer to this whole process as being [pseudo-random](https://en.wikipedia.org/wiki/Pseudorandom_number_generator).

OK cool, so what value does it have? The main advantage to wanting to include PCG in a project would be to have the quantity of content to support the product. The vast amount of interactive content that could be seemingly infinite allows for much replayability in a game such as in Spelunky, where it needs it in order to succeed. If it didn't have the thrill of not knowing what is down a hole, or what item to carry forwards, it would make all the levels static and boring. You can't easily nor fairly speedrun Spelunky. Mark covers Spelunky and it's use quite well in this video...

{% include video id="Uqk5Zf0tw3o" provider="youtube" %}

## PCG Versus Hand Authored

Architecting a project with PCG makes it a cornerstone for meeting at least one of the pillars from the game design document, usually based around longevity and combined with other large quantitive factors such as weapons, consumables, armour sets, enemies and traps. You can begin to visualise across genres such as roguelikes, RPGs and adventure titles that this data driven pattern can facilitate player retention quite well, whilst the developer can continue to satiate the desire a gamer could want post release. Although not PCG, Candy Crush Saga is a shining example of providing content with it's (as of writing) 8450 levels, with a finely crafted user flow for monetisation. The human cost of producing each of these levels *(whilst grueling no doubt)* is small enough to become viable. This usually isn't the case with the majority of other more compelling games, particularly 3D ones. Due to the nature of balancing a puzzle design and flow of each level, it wouldn't be an appropriate choice to generate all those levels procedurally.

Take a hand authored game such as Overwatch. Developing a new character, whole map or even just a seasonal skin is a relatively expensive epic task, even if art is outsourced. Implementation, testing and balancing will need to be thorough to ensure it goes out good enough before an inevitable hotfix comes in. Maintaining this model doesn't get any cheaper over time, and as new projects come into the studio, content delivery may need to slow down. This hand crafted love shows this and is the most appropriate method for making a game like that. I'm just comparing project architecture on a high level here as it's the end result of the game design which influences the project implementation method(s).

## Combining PCG *and* Hand Authored

It takes a specifically designed game to even be allowed to benefit from PCG. Another 2D example which should be showcased is Dead Cells. Sebastien talks more about their usage of it below.

{% include video id="tyMrRW-Li_I" provider="youtube" %}

What has evolved in this example over Spelunky tooling wise? It's the concepts of different room types and biomes. So the map looks more organic than just 16 box rooms with an entrance and an exit. There are corridors and event rooms. These event rooms can be shops or teleporter areas etc. This layer provides a hand crafted feel per room piece type of beauty in what is a seemingly random arrangement. Biome theming is the idea of maintaining everything to be functionality the same, with only the visuals and audio to change so that you're looking at a whole lot more of viable content.

That deterministic tree of outcomes has grown exponentially, when layered with a mix of player journey flow possibilities and a different game feel through aesthetics. [Dead Cells does this wonderfully](https://www.gamasutra.com/blogs/SebastienBENARD/20170329/294642/Building_the_Level_Design_of_a_procedurally_generated_Metroidvania_a_hybrid_approach.php), and it shines to make you not recognise that an algorithm generated the world you play on each run. The room stitching is seamless and there isn't distinct room repetitiveness. To the layman player, they wouldn't notice that the worlds are constructed this way *(mission accomplished)*. From a developer's perspective, with enough layering, we can create both high quantity *and* quality of levels, which gameplay wise allows for success at placing the emphasis on the player's instincts and reflexes than relying on learning level layouts for a roguelike like Dead Cells.

## Does it Work In 3D Too?

The examples discussed so far have all been based around 2D games. The application to 3D varies pending the game's genre. Lets discuss proceduralism in textures as a base first. Below is a screenshot of 4 different procedural noise algorithm nodes in Substance Designer.

![Alt](\assets\images\2020-12-27-dungeon-maker-1\noises.png "Procedural noise examples")

[Perlin](https://en.wikipedia.org/wiki/Perlin_noise) being a classic staple mathematic go to noise (ancestor to [Simplex](https://en.wikipedia.org/wiki/Simplex_noise)), with many terrains being founded with this algorithm. When layered with coherent math and a good artist, we can seemingly replicate mountainous ranges in a height map which isn't laboriously hand sculpted. This is a great use of PCG, as ordered chaos is all around us in our day to day, from soil in plant pots to water drops in a stainless steel sink. The below image demonstrates the logic of creating ordered chaos *(or chaotic order depending on which end of the telescope you choose to look through)*.

![Alt](\assets\images\2020-12-27-dungeon-maker-1\blendedNoise.png "Chaos and order together")

Exposing parameters such as range clamps can allow the end user to refine terrains in appropriate ways. When we begin to layer on more properties of terrains such as gorges, rivers and base rubble deposits, we can see how we can bring them to life. This is an art driven application, (with design driven influences of course) and is the foundation of software such as [Gaea](https://quadspinner.com/) and [World Machine](https://www.world-machine.com/).

In a more game integrated sense we have [No Man's Sky](https://store.steampowered.com/app/275850/No_Mans_Sky/), with procedurally generated planets. Upon launch it made news of how it failed to meet the quality expectations of PCG due to the behemoth of a project it is, yet over time they've released free updates to ascend it to a quality product *(with years worth of updates mind)* which makes their core product pillar sing. Yes, it's expensive to incorporate PCG into a 3D game as it becomes the backbone of the project, and arguably takes a lot longer for it to have an accepted quality bar over a 2D product with similar intent. This is why we see PCG being used partially in 3D in a lighter sense for tasks such as tooling light posts and telegraph poles along splines in open world editors in Far Cry. Whilst that initial spline still has to be drawn in engine by a user, it's in the same realm on PCG tooling, as the tool needs to consider trees, vegetation biomes, roads and pathways when being drawn. Ultimately, isn't a PCG planet generator just another tool?

## Input Models

Without going the PCG rabbit hole of procedural modelling and texturing, let's focus the use case on gameplay, such as how the Borderlands series uses PCG to generate near countless weapon combinations. They have 9 class defining manufacturers, layered with modifiers of different scales influenced to the player level, rarity, environment area and so forth. Factors determine the probability that they'll receive a weapon of what parameters with the core data (or weapon makeup) being stored in a database. Spreadsheets are a logical common solution for databases, and easily edited by multiple developers using a cloud based provider *(cough Gsheets)*. Their downside is that all data has to be injected, and it makes referencing in engine content, sometimes trickier than it may need to be than if it was done for a non PCG game.

However, whilst databases paired with in game probability logic sounds like the dynamic duo for an input model, there are different methods of PCG which can be approached for level design. With dynamically changing dungeons via level designers, an in engine implementation may be required. One where a team can collaborate and produce levels without external tools or data. Throughout this series I'll sharing my journey of procedural 3D dungeon design, focused around this idea.

## Is PCG Right For Your Game?

Is your design data driven? Can you call upon a vast array of variables which will make gameplay last longer and add value? This isn't an idea you can crowbar in like a plugin, it's a design philosophy which if done wrongly will be a time sink and cause you to not release a product at all. If the quality suffers too much, or you're doing something narrative driven, look at how PCG tooling can assist the process instead and not define your core loop.

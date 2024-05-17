---
layout: single
title:  "Bounties of Babylon - Post Mortem"
excerpt: "I made a commercial game in 7 months. Read about my full journey."
categories: [project]
toc: true
---

Introducing Bounties of Babylon!
This is the game I released on Steam after working on it full time for 7 months.

<iframe width="560" height="315" src="https://www.youtube.com/embed/Q-3QJCdEYkA" frameborder="0" allowfullscreen></iframe>
---

<iframe src="https://store.steampowered.com/widget/2712650/" frameborder="0" width="646" height="190"></iframe>

## Starting off - what to make?
Discovering "the problem" with luxury products (which all entertainment products and services are), is a much trickier task than businesses that are more quantifiable with what their aim is. You're now competing for the user's time with all entertainment products that they could choose to spend their time on. Do they watch Netflix, play a game or read a book? Fintechs improve financial solutions. Fitness apps accompany health awareness. Games improve... happiness?

The problem is that happiness _(and the theory of fun)_ is different to everyone. A lot of product creators ask "well, what does that look like to me?" In a male dominant driven industry, history has shown that for PC/Console, is that a lot of titles trend to more motor driven, gore/military based themes because of the key demographics. It is refreshing to see emerging PC genres such as "cozy", where a colourful zen experience and creativity building are considered key genre tropes, and be profitable too (for the larger hitters).

I knew I didn't want to make a clone as I value originality, and enjoy cognitive based games. Despite some yellow flags of articles written on [HTMAG.com](https://howtomarketagame.com/blog/) saying not to do board games / very niche strategy titles, I saw that there were a lot of overwhelmingly poor reviews of the popular board game _Catan_ on Steam, for poor implementation reasons. A large audience was established looking for some hex turn-based fun, so this was the potential market opportunity I decided to go after, knowing I'd also enjoy the process.

## Don't compete with Netflix
People are far more likely to play AAA hits like _God of War_ in their lounges at home, So by reducing the product's viable competition and focusing on the quicker session based mechanics helped make the product more approachable as an indie developer. Cinematics and story would only cloud it's scope.

Many people also want to chill and watch content when exhausted. Cognition is exhausting, so when making a game, I consider the aspect of making it digestible in a timely manner. Can a user consume it for a shorter period of time, ideally in a mobile format? Not that I'm suggesting a match-3 mobile game, but occupying the attention slots they occupy. This enables you to have entertainment in spaces where you can't achieve the wow-factor that cinematic experiences provide. Transport, the loo and waiting rooms are all appealing opportunities for a _Bounties of Babylon_ session.

![Alt](\assets\images\2024-05-17-bounties-of-babylon-post-mortem\gallery-on-deck.png)

Ultimately, this is why I chose to develop with the Steam Deck so early on, where I enjoyed playtesting _Bounties of Babylon_ whilst sat in bed.

## The initial design
In 2017 I made a physical prototype called _Hex Chess_. Each piece was a tile card that had an ability, with it's own movement pattern set on a hexagonal chess board. You rolled a weather die which dictated the state of the board on your turn (frozen = pieces can't move, sun/moon = piece affiliation buffs, etc...); and each chess piece had it's own ability and movement pattern. As chess is purely deterministic with no luck, this was my attempt at rubber banding the skill curve closer for lesser skilled players.

Based on my now apparent obsession with hexagons for tactical mapping, I blended _Catan_ for it's resource collection and trading with _Carcassonne_'s procedural tile placement and map building to create a premise with a lot of replayability.

The Silk Road made the ideal premise. The rivers were originally roads and the ships were originally carts. There was even _Meeple_ involved for merchant encounters at one point too. The naval themed tradable artifacts in the game were originally very Asian/Turkish in theme, such as oriental snuff bottles and silk cloth itself.

## Core systems
Implementing an axial coordinate system was the first port of call. The tile state machine with _road_ connections needed to occupy available slots to form a network. Tile placement with legal orientation checking was paramount to get right at this stage, as it'll set the stage for path finding for traversal and bounty resource collection later on. In many respects, it's comparable to making an inventory system.

![Alt](\assets\images\2024-05-17-bounties-of-babylon-post-mortem\prototype.png)

__Validation is king!__ I wrote a lot of it, and definitely don't regret it. Levels are prepopulated with a variety of data, and each field needed to be 100% error proof. Thankfully with custom editor tooling I was able to ensure that `RoundData` and it's children data were always valid (or throw errors when it becomes invalid). Aside from your standard range clamps, strict object types and valid initalisation, I wrote various counters which can inform other properties if validation becomes compromised.

There's an interface called `IQuest` which contracts each quest-based scriptable object to transact it's shared data. I prototyped with 2 quests for quite some time before I scaled nearer the end of the project. By reducing the quantity of data to refactor throughout feature development, time wastage can be prevented. Quests involve checking data at certain points and calling back via delegates to the `QuestController`. I think this goes in hand with quest design. Use the data you have available and design combinations to create scalable variety. Change requirement quantities and reward values, and you have yourself a dynamic quest system.

## The public playtest
At the start I wrote a GDD and technical roadmap, which up until now was a very valuable tool in mapping out the project's feature foundations. There are still ideas on there which I would have liked to explored, but now 3-4 months in, the systems and base data are flowing well and extendable.

Now it's up to the users to see how the product evolves from the first playtest. With a Qualitative feedback form and dashboard data on playtime, I was able to get an insight on how players responded to the game. This was where the GDD devolved from a blueprint guide into an advisory document.

I learnt that I had a viable product, but it wasn't permanently engaging due to the "pass and play" multiplayer aspect. That's when I knew I had to make it a single player game and incorporate AI logic to play against. It's a task I delayed until this feedback confirmed (as the most requested feature) that I need a snappier core loop for a better quality experience.

## CPU AI and data management
There is a finite state machine for progressing through the turns, using the factory design pattern. Each phase of the turn is represented by a state. Previously, this machine would loop through depending on the player turn.

With a CPU AI, I injected new states which would process it's own automated logic, and merge back for parallel logic. It would be a poor design decision to duplicate states which are similar. It simply isn't SOLID. For near similar states such as the dice roll, which only swap the die colour, small injections of variable checking and property swapping are appropriate.

![Alt](\assets\images\2024-05-17-bounties-of-babylon-post-mortem\turn-fsm.png)

Steam Cloud was used at key stages throughout the turn (so as not to spam requests). The more granular session data management (and core workhorse), was the instance data manager.

For anyone looking for a C# wrapper for Steamworks API, I wholeheartedly recommend the [Facepunch API](https://wiki.facepunch.com/steamworks/) for it's reliability, accuracy, wrapper translation, and it's well documented.

## Steam Next Fest
The Steam page had only been live for 2-3 months by this point, premature in marketing terms and I knew my wishlist count wasn't high enough to get a big boom from it, but here are my key takeaways:

1. Your demo has to look final quality without any bugs. Consumer expectations are high.
2. Use [Robostreamer](https://robostreamer.com/) to loop prerecorded auto-marketing content all week.
3. Get your two video slots in early in the week due to less activity near the end of the fest.
4. Have a healthy following before you go into the fest.
5. Localisation doesn't matter at this point. I had my build localised in 12 languages but the majority of players were English speaking.

## Closing thoughts
Releasing your Steam store page and announcing your game was a larger marketing beat than I had anticipated. Especially with how Steam pushes you in front of the discovery queue so early on.

- Did I enjoy the entire process? Yes.
- Was it hard work and a labour of love for 7 months? You bet.
- Would I make a strategy game again? For sure, but I'd go about it differently.

With ~30 games entering Steam's marketplace every day (as of 2024), it is harder than ever to be discovered in the marketplace. Since the post Covid layoff (and still continuing) fallout, the business has changed remarkably. Budgets are tighter, teams are smaller, and tool selection has never been so accessible. It is becoming harder to innovate in this space and make something worth turning heads for, but with the right factors it's still a viable space.

I'm proud to have embodied the full process as a solo developer on my own commercial title.

---
title: "NPC spawns in Yu-Gi-Oh! Nightmare Troubadour"
date: 2021-07-09 20:39:39 +0100
categories: [Yu-Gi-Oh! Nightmare Troubadour]
---
[Last post][previous-post] we looked at the details of how packs work in
[Yu-Gi-Oh! Nightmare Troubadour][nightmare-troubadour]. In this post we're going
to look at how the game decides which NPCs to spawn, and a little about how the
game places them on the map. Like the last post, this work was done on the PAL
version of the game. I did have a quick peek at the corresponding data for the
NTSC-U version though, because of the issue with Pegasus.

## Overview

Yu-Gi-Oh! Nightmare Troubadour does not merely present the player with a list
of opponents to duel at any time they wish. Instead, the opponents exist on an
overworld and must be found by the player. This can be seen as an evolution of
the system introduced in [Duel Monsters 6: Expert 2][duel-monsters-6].

![A player searching for opponents](/assets/img/ntr-first-city.png)
_Konami's take on Hot and Cold_

The player moves a cursor around the city they're in (past a certain point the
player unlocks a second city). The cursor starts as blue, but when it gets close
to an opponent it turns green. Closer still, it turns red. Finally when it's
very close the opponent will be detected and you have the option to face them.
The amount of information you have on who an opponent is depends on details we
need not go into here.

The opponents who have a chance of appearing depend on which of the two cities
the player is in, what time of day it is (there is an in-game day/night cycle),
and how far the player has progressed through the story. So first question, who
can appear and when?

## Who wants to play?

Time in Nightmare Troubadour is a value from 0 to 255. The symbol the game uses
for time changes for each quarter of the day, but the game additionally
subdivides each quarter into four smaller quarters. The sixteenth of the day
affects which opponents can spawn.

![Images showing each sixteenth of the day](/assets/img/ntr-times-of-day.png)
_The earliest time of each sixteenth of the day_

Progress through the story is represented by two values, a stage and a substage.
The stage goes from zero to ten; the substage's range of values depends on the
current stage. I have not spent the time to work out the conditions that must
be met to progress to the next stage or substage. Some educated guesses are
possible, and I suspect the simplest way to work out the troublesome ones would
be to experiment with savestates while looking at the relevant values. The stage
is a two-byte value at 0x2097460, while the substage is the byte at 0x2097464.

Using these values, the game builds the list of opponents that can spawn subject
to those conditions. I've put the data into [this XML file][pal-opponents].
Each element has the time, stage, substage, and the city corresponding to the
data.

The next important thing is the PRNG used. Like with packs, it's an LCG, but
it's a different LCG. I have included an auxiliary method called `randnum`
that gives a procedure the game often uses to generate a random number from
0 to upper_bound - 1 for some upper bound.

```python
class Prng:
    def __init__(self, seed):
        self.__seed = seed

    def rand(self):
        self.__seed = (0x41C64E6D * self.__seed + 0x3039) & 0xFFFFFFFF
        return (self.__seed >> 16) & 0x7FFF

    def randnum(self, upper_bound):
        return (self.rand() * upper_bound) // 32768
```

It appears the game initialises the seed with the number of frames elapsed since
power-on when the player clicks on Continue. I haven't thoroughly tested it but
it doesn't appear to be called every frame or anything like that, so manip
should be very viable. Whether it'd be worthwhile is another matter.

With the PRNG set up, the game looks at a base count for the number of opponents
to spawn. This only depends on the city, stage (not substage), and time, and is
included in the XML as `baseCount`. The game generates a random number from 0 to
`baseCount` - 3, then adds 3, so it's a random number from 3 to `baseCount`.
This is with the exception of the case where `baseCount` is 2, in which case it
is instead always 3. This random number is meant to be how many opponents the
game spawns.

If the number of opponents to spawn is at least as great as the possible number
of opponents, the game spawns all of them. Otherwise it randomly selects the
desired amount out of the list of possible opponents. For this purpose, each
of the possible opponents comes with a 'spawn rate', and roughly speaking a
higher spawn rate means a higher chance of the opponent being selected. Some
Python giving how the game goes about the selection is given below.

```python
def select_available_opponents(prng, opponent_list, base_count):
    # opponent_list is an array of (opponent_name, spawn_rate)
    count = prng.randnum(base_count - 2) + 3
    if count >= len(opponent_list):
        return [opp for opp, _ in opponent_list]
    opponents = []
    # This copy is so we can safely modify the local list
    opponent_list = [x for x in opponent_list]
    total_spawn_rate = sum(prob for _, prob in opponent_list)
    while len(opponents) < count:
        slot = prng.randnum(len(opponent_list))
        while True:
            character, char_prob = opponent_list[slot]
            threshold = (char_prob * 100) // total_spawn_rate
            if prng.randnum(100) < threshold:
                break
            slot = (slot + 1) % len(opponent_list)
        opponents.append(character)
        opponent_list = opponent_list[:slot] + opponent_list[slot+1:]
    return opponents
```

## Where shall we play?

This is only half of the puzzle. Once the game has decided who to spawn, the
game then has to decide where on the map to place them. I have not worked out
all the details of this process, but I have enough to be worth documenting.

The game goes through the spawned opponents in order of their ID, rather than
the order returned from the above procedure. This order can be inferred from the
XML, where the available opponents start ordered by their ID. For each one, the
game generates a position for them. It does this by continually generating
random points until it finds one far enough away from all previously placed
opponents, the player, and it seems any visible buildings on the map. This
includes the train station, card shop, the player's home, the Duel Base, and
probably places like the location of the Expert Cup finalists party and the
building Serenity is taken to. There is an additional check on the generated
location, which I believe is how the game makes sure to not spawn opponents in
the middle of the sea.

![A player searching for opponents in the sea](/assets/img/ntr-sea.png)
_Poseidon does not care to duel you_

The function that keeps generating a random point until a valid one is found
is called FUN_overlay_d_15__022630c0 by Ghidra, in case anyone wishes to
investigate further.

## Loose ends: passersby and Shadow Games

I have not looked much into passersby or duelists who intercept you and force
you into a Shadow Game, but it probably isn't complicated. For passersby, it
appears the game may add one after assigning a position to all spawned duelists,
with a spawn rate of 0%, 20%, or 100%. If one is spawned, they are assigned a
position the same way duelists are.

In the function that takes care of selecting which opponents to spawn, there is
a branch for the case where the game wants to spawn an opponent to challenge you
to a Shadow Game. It goes the same as `select_available_opponents` usually does,
except `count` will always be one and not consult `base_count`.
[Here][shadow-game-opponents] is a similar XML for such opponents.

## Pegasus

It is well-known amongst players of Nightmare Troubadour that there is a bug
preventing Pegasus from spawning in the NTSC versions once Odion has been
defeated for the first time. This means that if not already obtained, the player
is locked out of Pegasus' Deck Recipe and Imperial Order.

I had a quick glance at the corresponding code for the NTSC-U version and found
no relevant differences. However, there are differences in the data giving which
opponents can spawn and therein lies the answer. I have placed the NTSC-U data
[here][ntsc-opponents] for convenience. The only difference is Pegasus is not
listed as spawning in stage ten at all, and in any city/time/substage
combination in which he spawns in stage ten, he is removed and base_count is one
lower. As a curious consequence, note that this still means that he is not
present for stage nine, which is probably from after the player first defeats
Odion up until Yami Marik is defeated.

There is no difference in the data for opponents who intercept the player and
force them into a Shadow Game.

[duel-monsters-6]: https://yugipedia.com/wiki/Yu-Gi-Oh!_Duel_Monsters_6:_Expert_2
[nightmare-troubadour]: https://yugipedia.com/wiki/Yu-Gi-Oh!_Nightmare_Troubadour
[ntsc-opponents]: {% link assets/data/ntr-opponents-ntsc.xml %}
[pal-opponents]: {% link assets/data/ntr-opponents-pal.xml %}
[previous-post]: {% post_url 2021-06-24-ntr-packs %}
[shadow-game-opponents]: {% link assets/data/ntr-shadow-game-opponents.xml %}

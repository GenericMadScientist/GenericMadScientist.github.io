---
title: "Challenge Cup teams in Pokémon Stadium 2"
date: 2022-01-09 12:34:30 +0000
categories: [Pokémon Stadium 2]
---
It's been a while, hasn't it? I did have plans for having my next post be on
[Dragon Ball Z: Budokai Tenkaichi 2][dbz-bt2], but then I decided that to do
that as well as I wanted I would first have to do one on
[Dragon Ball Z: Budokai 3][dbz-b3] and I quickly lost interest during my
playthrough of that.

But enough excuses! Today we're going to dive into
[Pokémon Stadium 2][stadium-2]'s Challenge Cup mode. For the sake of
specificity, this was worked out on the NTSC-U version of the game.

## Overview

Pokémon Stadium 2 is a game principally focused on battling. There are various
cups, gyms, and a few other fights here and there. For most of them you get to
use either your own Pokémon from the Game Boy and Game Boy Color games, or use
rentals the game provides you with.

The Challenge Cup is special though, in that both you and your opponent have
randomised teams. There are four divisions, called Poké Ball, Great Ball, Ultra
Ball, and Master Ball. Each division has their own pool of types of Pokémon that
can be used, with higher divisions generally offering stronger species. There is
also a Round 2 mode to the game that unlocks after you finish all the battles,
where you do everything again with harder opponents. In Round 2 the opponent's
Pokémon have higher stats, and the randomly generated Pokémon have access to a
wider move pool.

These randomly generated teams are going to be the focus of this post, both the
player's and the AI's. I will just be describing the main ideas without caring
about the specific order the game actually makes all these decisions and calls
the RNG. If you want that level of detail, I have written a replica of how the
game does it which you can play with [here][cc-sim]. The repository is
[here][cc-sim-repo].

## Species selection

The four divisions each have their own pool of allowed species. These are:

| Poké Ball  | Great Ball | Ultra Ball | Master Ball |
| ---------- | ---------- | ---------- | ----------- |
| Bellsprout | Abra       | Arbok      | Aerodactyl  |
| Bulbasaur  | Aipom      | Bellossom  | Alakazam    |
| Charmander | Ariados    | Chansey    | Ampharos    |
| Chikorita  | Azumarill  | Charmeleon | Arcanine    |
| Cleffa     | Bayleef    | Clefable   | Blastoise   |
| Cyndaquil  | Beedrill   | Dewgong    | Blissey     |
| Diglett    | Butterfree | Dragonair  | Charizard   |
| Ditto      | Chinchou   | Dugtrio    | Cloyster    |
| Dratini    | Clefairy   | Fearow     | Crobat      |
| Drowzee    | Corsola    | Forretress | Dodrio      |
| Ekans      | Croconaw   | Furret     | Donphan     |
| Exeggcute  | Cubone     | Girafarig  | Electabuzz  |
| Geodude    | Delibird   | Gligar     | Electrode   |
| Goldeen    | Doduo      | Golbat     | Espeon      |
| Grimer     | Dunsparce  | Granbull   | Exeggutor   |
| Hoothoot   | Eevee      | Haunter    | Feraligatr  |
| Hoppip     | Elekid     | Hitmonchan | Flareon     |
| Horsea     | Farfetch'd | Hitmonlee  | Gengar      |
| Igglybuff  | Flaaffy    | Hitmontop  | Golduck     |
| Jigglypuff | Gastly     | Hypno      | Golem       |
| Krabby     | Gloom      | Jumpluff   | Gyarados    |
| Larvitar   | Graveler   | Kadabra    | Heracross   |
| Ledyba     | Growlithe  | Kingler    | Houndoom    |
| Machop     | Houndour   | Lanturn    | Jolteon     |
| Magnemite  | Ivysaur    | Magneton   | Jynx        |
| Mareep     | Kabuto     | Mantine    | Kabutops    |
| Marill     | Koffing    | Marowak    | Kangaskhan  |
| Nidoran♀   | Ledian     | Misdreavus | Kingdra     |
| Nidoran♂   | Lickitung  | Mr. Mime   | Lapras      |
| Oddish     | Machoke    | Murkrow    | Machamp     |
| Paras      | Magby      | Noctowl    | Magmar      |
| Pichu      | Magcargo   | Octillery  | Meganium    |
| Pidgey     | Mankey     | Persian    | Miltank     |
| Pineco     | Meowth     | Pidgeot    | Muk         |
| Poliwag    | Natu       | Piloswine  | Nidoking    |
| Rattata    | Nidorina   | Ponyta     | Nidoqueen   |
| Remoraid   | Nidorino   | Pupitar    | Ninetales   |
| Sandshrew  | Omanyte    | Quagsire   | Omastar     |
| Seel       | Onix       | Quilava    | Pinsir      |
| Sentret    | Parasect   | Qwilfish   | Politoed    |
| Shellder   | Phanpy     | Raichu     | Poliwrath   |
| Slowpoke   | Pidgeotto  | Raticate   | Porygon2    |
| Slugma     | Pikachu    | Sandslash  | Primeape    |
| Smeargle   | Poliwhirl  | Seadra     | Rapidash    |
| Snubbull   | Porygon    | Seaking    | Rhydon      |
| Spearow    | Psyduck    | Shuckle    | Scizor      |
| Spinarak   | Rhyhorn    | Skarmory   | Scyther     |
| Squirtle   | Skiploom   | Slowbro    | Snorlax     |
| Sunkern    | Smoochum   | Slowking   | Starmie     |
| Swinub     | Staryu     | Sneasel    | Steelix     |
| Togepi     | Teddiursa  | Stantler   | Tauros      |
| Totodile   | Tentacool  | Sudowoodo  | Tentacruel  |
| Tyrogue    | Togetic    | Sunflora   | Typhlosion  |
| Venonat    | Voltorb    | Tangela    | Umbreon     |
| Vulpix     | Wartortle  | Venomoth   | Ursaring    |
| Wooper     | Weepinbell | Vileplume  | Vaporeon    |
| Zubat      | Wobbuffet  | Weezing    | Venusaur    |
|            | Yanma      | Wigglytuff | Victreebel  |
|            |            |            | Xatu        |

To select the species, the game starts with a template. A template is a list of
either Pokémon types or wildcards. For the player, the game randomly selects
one of the following six templates:

* Electric / Ground / Water / \* / \* / \*
* Electric / Fire / Water / \* / \* / \*
* Rock / Grass / Water / \* / \* / \*
* Electric / Ground / Flying / \* / \* / \*
* Electric / Ground / Fire / \* / \* / \*
* Rock / Fire / Grass / \* / \* / \*

Each opponent also has two templates (since you face each opponent in two
divisions), using the same templates in Round 1 and Round 2. These are:

| Poké Ball         |
| ----------------- |
| Camper Marcus     | Fire     | Fire     | Ground   | Ground | Ground | Ground |
| Rocket ♂          | Poison   | Poison   | Poison   | Poison | Poison | Poison |
| Picnicker Melissa | Flying   | Flying   | Flying   | Flying | \*     | \*     |
| Guitarist Daren   | Electric | Electric | Electric | Grass  | Grass  | Grass  |
| Fisherman Curtis  | Water    | Water    | Water    | Water  | Water  | Water  |
| Medium Peggy      | Psychic  | Psychic  | Psychic  | \*     | \*     | \*     |
| Rocket ♀          | Normal   | Normal   | Normal   | Normal | Normal | Normal |
| Juggle Dwight     | \*       | \*       | \*       | \*     | \*     | \*     |

| Great Ball        |
| ----------------- |
| Twins Jan & Jane  | Bug    | Bug    | Bug     | Bug     | \*    | \*    |
| Schoolboy Oliver  | Ground | Ground | Rock    | Rock    | Rock  | Rock  |
| Sailor Curt       | Ice    | Water  | Water   | Water   | Water | \*    |
| Swimmer♀ Darcy    | Normal | Normal | Normal  | Normal  | \*    | \*    |
| Officer Gerald    | Fire   | Fire   | Fire    | Grass   | Grass | Grass |
| Kimono Girl Emiko | \*     | \*     | \*      | \*      | \*    | \*    |
| Scientist Roberto | Dark   | Ghost  | Psychic | Psychic | \*    | \*    |
| Gentleman Travis  | \*     | \*     | \*      | \*      | \*    | \*    |

| Ultra Ball        |
| ----------------- |
| Camper Marcus     | Fire     | Fire     | Ground  | Ground  | Ground  | \*     |
| Rocket ♂          | Poison   | Poison   | Poison  | Poison  | \*      | \*     |
| Picnicker Melissa | Flying   | Flying   | Flying  | Flying  | Flying  | \*     |
| Guitarist Daren   | Electric | Electric | Grass   | Grass   | \*      | \*     |
| Fisherman Curtis  | Water    | Water    | Water   | Water   | Water   | Water  |
| Medium Peggy      | Ghost    | Ghost    | Psychic | Psychic | Psychic | \*     |
| Rocket ♀          | Normal   | Normal   | Normal  | Normal  | Normal  | Normal |
| Juggle Dwight     | \*       | \*       | \*      | \*      | \*      | \*     |

| Master Ball       |
| ----------------- |
| Twins Jan & Jane  | Bug      | Bug      | Bug     | Flying  | Flying | \*    |
| Schoolboy Oliver  | Steel    | Ground   | Ground  | Ground  | Rock   | Rock  |
| Sailor Curt       | Ice      | Ice      | Water   | Water   | Water  | \*    |
| Swimmer♀ Darcy    | Fighting | Fighting | Normal  | Normal  | Normal | \*    |
| Officer Gerald    | Fire     | Fire     | Fire    | Grass   | Grass  | Grass |
| Kimono Girl Emiko | \*       | \*       | \*      | \*      | \*     | \*    |
| Scientist Roberto | Dark     | Ghost    | Psychic | Psychic | \*     | \*    |
| Gentleman Travis  | \*       | \*       | \*      | \*      | \*     | \*    |

The game generates a Pokémon for each element of the template. To do this, it
creates a list of eligible Pokémon and then selects one at random. This is done
in two steps.

Firstly the game looks at the template element. If it is a type, the game takes
all the Pokémon eligible for the division with that type that are not already in
the team. If it is a wildcard, the game takes all Pokémon eligible for the
division that do not share a type with one already in the team. However, there
is something of note with this: if a Pokémon is dual-typed and doesn't share a
type with anything already in the team, it gets added to the list twice.

The second step is done by the game to try and keep the base stat total average.
The game looks at the average base stat total of the current team members and
rounds it down (if this is the first Pokémon so the team is empty, the average
is treated as 0). The game then divides the current list of candidates into two
lists: one of Pokémon with a base stat total below the team average, and one
with those that have a total at least as great as the average.

The game has target average base stat totals for each division, and these are
280, 360, 447, and 507. These turn out to be the average base stat total for all
Pokémon eligible in each division, rounded to the nearest integer. The target is
used to select which of the two lists to go with: if the target is below the
team average, it will go with the below average list, else it will go with the
at least average list. However, if the list chosen this way turns out to be
empty, it will fall back on the other non-empty list instead.

From this it is clear some opponents will always have specific Pokémon because
they ask for all of the eligible Pokémon with a given type. For example, in
Ultra Ball Peggy's template asks for two Ghosts. The only Ghosts in Ultra Ball
are Haunter and Misdreavus, so she will always have these.

This type of thing can happen in a less obvious way as well though, like with
Roberto in Great Ball. There is only one Dark and one Ghost in Great Ball, so
he will always have Houndour and Gastly. Up to this point his average base stat
total is thus 320. Now there are four eligible Psychics in Great Ball: Abra,
Natu, Smoochum, and Wobbuffet. Only Natu and Wobbuffet have a base stat total
of at least 320. If the game gives him Natu for the first Psychic, then his
average will still be 320 so he will be forced to have Wobbuffet as his fourth
team member. Thus, no matter what, Great Ball Roberto will always have
Wobbuffet.

## Moves

Ditto and Wobbuffet are special for move selection. Ditto always just has
Transform, and Wobbuffet always has Counter, Destiny Bond, Mirror Coat, and
Safeguard. For every other Pokémon, the following happens.

To decide the moves for each Pokémon, the game first creates a list of every
move the Pokémon can learn up to its level (which is 30, 45, 60, and 75 for the
four divisions) using the following procedure.

If the Pokémon is Smeargle, it starts by adding every move except Baton Pass,
Metronome, Mimic, Mirror Move, Sketch, Sleep Talk, Struggle, and Transform.
Next, it will add every move the Pokémon can learn from level-up in Gold and
Silver up to its level. If the Pokémon is unevolved and can hatch from an egg,
it will treat its level as 100 for this check. Then all moves that it can learn
from a TM or HM in Gold and Silver are added.

If it is Round 2, the game will add additional moves to the learn pool. These
are: egg moves, Crystal move tutor moves, Crystal level-up moves, RBY level-up
moves, and RBY TM and HM moves. Note this does not include some legal moves like
ExtremeSpeed on Dratini and Amnesia on Psyduck.

If the Pokémon is evolved, it will then repeat this procedure with its
pre-evolution and add all the moves thus obtained. If the evolution is via
level-up then the level will be decreased by 1 for this consideration, otherwise
it is unchanged. However for this purpose, Tyrogue's evolutions are not
considered as being by level-up.

Next, the following moves are removed: Absorb, Bind, Bubble, Constrict, Dream
Eater, Fire Spin, Fissure, Frustration, Guillotine, Hidden Power, Horn Drill,
Leech Life, Lick, Milk Drink, Moonlight, Morning Sun, Nightmare, Poison Sting,
Rage, Rapid Spin, Recover, Rest, Return, Rock Smash, Sketch, Sleep Talk, Smog,
Snore, Softboiled, SonicBoom, Splash, Struggle, Synthesis, Teleport, Whirlpool,
and Wrap.

Some opponents then have their own signature moves. These are:

* Fisherman Curtis: Rain Dance
* Guitarist Daren: Glare, Stun Spore, Thunder Wave
* Medium Peggy: Confuse Ray, Supersonic, Swagger, Sweet Kiss
* Officer Gerald: SolarBeam, Sunny Day
* Rocket ♀: Bite, Bone Club, Headbutt, Hyper Fang, Low Kick, Rock Slide, Rolling
  Kick
* Rocket ♂: Poison Gas (Poké Ball only), PoisonPowder (Poké Ball only), Toxic
* Schoolboy Oliver: Sandstorm
* Scientist Roberto: Double Team, Flash, Kinesis, Minimize, Sand-Attack,
  SmokeScreen, Sweet Scent
* Swimmer♀ Darcy: Attract

Any signature moves for the opponent that are in the move pool are removed and
added to a special list. What remains is divided into four main classes of moves,
whose members are given below.

Good support moves: Acid Armor, Agility, Amnesia, Attract, Barrier, Belly Drum,
Charm, Confuse Ray, Cotton Spore, Destiny Bond, Double Team, Growth, Haze, Heal
Bell, Light Screen, Lock-On, Mind Reader, Perish Song, Rain Dance, Reflect,
Safeguard, Sand-Attack, Scary Face, Screech, SmokeScreen, Spikes, Spore,
Substitute, Sunny Day, Swagger, Sweet Scent, Swords Dance, Thunder Wave, Toxic

Bad support moves: Baton Pass, Conversion, Conversion 2, Curse, Defense Curl,
Detect, Disable, Encore, Endure, Flash, Focus Energy, Foresight, Glare, Growl,
Harden, Hypnosis, Kinesis, Leech Seed, Leer, Lovely Kiss, Mean Look, Meditate,
Mimic, Minimize, Mirror Move, Mist, Pain Split, Poison Gas, PoisonPowder,
Protect, Psych Up, Roar, Sandstorm, Sharpen, Sing, Sleep Powder, Spider Web,
Spite, String Shot, Stun Spore, Supersonic, Sweet Kiss, Tail Whip, Transform,
Whirlwind, Withdraw

Good attacks: Aeroblast, AncientPower, Aurora Beam, Bite, Blizzard, Body Slam,
Bonemerang, BubbleBeam, Crabhammer, Cross Chop, Crunch, Dig, Dizzy Punch,
Double-Edge, DragonBreath, Drill Peck, Earthquake, Egg Bomb, Explosion,
ExtremeSpeed, Faint Attack, Fire Blast, Fire Punch, Flame Wheel, Flamethrower,
Fly, Giga Drain, Headbutt, Hi Jump Kick, Horn Attack, Hydro Pump, Hyper Beam,
Hyper Fang, Ice Beam, Ice Punch, Icy Wind, Iron Tail, Jump Kick, Magnitude, Mega
Kick, Mega Punch, Megahorn, Metal Claw, Octazooka, Outrage, Petal Dance,
Psybeam, Psychic, Razor Leaf, Rock Slide, Rollout, Sacred Fire, Selfdestruct,
Shadow Ball, Slash, Sludge, Sludge Bomb, SolarBeam, Spark, Steel Wing, Stomp,
Strength, Submission, Super Fang, Surf, Swift, Take Down, Thrash, Thunder,
ThunderPunch, Thunderbolt, Tri Attack, Twineedle, Vital Throw, Waterfall, Wing
Attack

Bad attacks: Acid, AncientPower, Aurora Beam, Barrage, Beat Up, Bide, Bite,
Bone Club, Bone Rush, Bonemerang, BubbleBeam, Clamp, Comet Punch, Confusion,
Counter, Cut, Dig, Double Kick, DoubleSlap, DragonBreath, Dragon Rage,
DynamicPunch, Egg Bomb, Ember, False Swipe, Faint Attack, Flail, Flame Wheel,
Fury Attack, Fury Cutter, Fury Swipes, Future Sight, Gust, Horn Attack, Hyper
Fang, Icy Wind, Jump Kick, Karate Chop, Low Kick, Mach Punch, Mega Drain, Mega
Kick, Mega Punch, Metal Claw, Metronome, Mirror Coat, Mud-Slap, Night Shade,
Octazooka, Pay Day, Peck, Pin Missile, Pound, Powder Snow, Present, Psybeam,
Psywave, Pursuit, Quick Attack, Razor Leaf, Razor Wind, Reversal, Rock Throw,
Rolling Kick, Rollout, Scratch, Seismic Toss, Selfdestruct, Skull Bash, Sky
Attack, Slam, Sludge, SolarBeam, Spark, Spike Cannon, Steel Wing, Stomp,
Submission, Swift, Tackle, Take Down, Thief, ThunderShock, Triple Kick,
Twineedle, Twister, Vine Whip, ViceGrip, Water Gun, Wing Attack, Zap Cannon

There is some overlap between good attacks and bad attacks; anything in said
overlap falls under both. Good and bad attacks are each further subdivided into
four groups: STAB attacks of the Pokémon's first type, STAB attacks of the
second type (if the Pokémon is dual-typed), non-STAB physical attacks, and
non-STAB special attacks.

With this set up, the moves can now be decided. For the first move, the game
picks a random element of a specific class of move, the first of the following
that has any moves at all:

* Good STAB attack (if dual-typed, one of the non-empty lists at random)
* Bad STAB attack (if dual-typed, one of the non-empty lists at random)
* Good non-STAB attack (one of the non-empty lists at random)
* Bad non-STAB attack (one of the non-empty lists at random)

After deciding the first move (and, in fact, after deciding the second and third
moves too) the game will first remove all copies of that move from all the lists
so it cannot be selected again. Then it will look at the added move and see if
it is the first move of a combo. If so, it will add multiple copies of the
second move of the combo to any lists said move is already in to make it more
likely. The combos and the number of copies added are as follows:

| First move   | Second move  | Copies |
| ------------ | ------------ | ------ |
| Giga Drain   | Growth       |      3 |
| Mega Drain   | Growth       |      3 |
| Zap Cannon   | Lock-On      |      4 |
| DynamicPunch | Mind Reader  |      4 |
| SolarBeam    | Sunny Day    |      3 |
| Thunder      | Rain Dance   |      3 |
| Outrage      | Safeguard    |      5 |
| Thrash       | Safeguard    |      5 |
| Petal Dance  | Safeguard    |      5 |
| Rollout      | Defense Curl |      7 |
| Reversal     | Endure       |      7 |
| Flail        | Endure       |      7 |
| Perish Song  | Mean Look    |      7 |
| Toxic        | Mean Look    |      7 |
| Swagger      | Psych Up     |      7 |

There are two additional combos in the game that, due to a bug, are ignored:
Toxic + Spider Web (7 copies) and Toxic + Pursuit (5 copies).

For the second move, the game first checks if the Pokémon is dual-typed. If it
is, the game tries to add a bad STAB attack of the type other than that of the
first added move. If there are no such options, it proceeds to the usual
procedure.

The usual procedure is to first combine the lists of good physical and special
non-STAB attacks and remove the ones that have the same type as the first chosen
move. If this results in a non-empty list, a move is randomly selected from this
list. However if this list is empty, the game picks at random either the list of
good non-STAB physical attacks or good non-STAB special attacks. If one is
empty, it just picks the non-empty one. Then a move is selected at random from
the chosen list.

For the third move, the game first checks if any of the opponent's signature
moves are available. If so, it will pick one at random as the third move. If
not, and there are any good support moves, it will first select one at random.
If the selected good support move is already on the team, it will do another
random pick of a good support move and go with that. This can be the one just
rejected, so it is still possible for a good support move to appear multiple
times on a team. If no good support moves are available, a random bad support
move is chosen instead.

For the final move, the game looks at the Pokémon's base stats. If the base
attack is higher, the game will take all bad non-STAB physical attacks that are
not of the same type as one of the first two moves chosen. If the base special
attack is higher, bad non-STAB special attacks will be used instead. If they are
tied, these lists are combined. The game then picks either this list or the list
of available bad support moves at random (or goes with the non-empty one if one
is empty) and picks a random move from it.

## Held items

With the moves chosen, the next step is the held item. The game will do the
below to decide a held item. If there is already a Pokémon on the team with that
held item, it will try again and again until it gets one that isn't.

There is a 116/256 chance the game will go for one of the following berry items,
chosen at random: Berry Juice, Bitter Berry, Burnt Berry, Gold Berry, Ice Berry,
Mint Berry, MiracleBerry, MysteryBerry, PRZCureBerry, or PSNCureBerry. If the
selection is PSNCureBerry and the Pokémon is Poison or Steel, the game tries
again as if a duplicate item were chosen. Similarly for Burnt Berry on Ice types
and Ice Berry on Fire types.

There is an 89/256 chance the game will opt for the held item that boosts the
power of attacks with the same type as the first move by 10%. If the type is
Normal, one of the Pink Bow or Polkadot Bow is chosen at random. If the type is
Dragon, one of the Dragon Fang or Dragon Scale is chosen at random. However if
the first move is one of Counter, Dragon Rage, Mirror Coat, Night Shade, Pain
Split, Psywave, Seismic Toss, or Super Fang, the game instead opts for the
previous option of a random berry item.

There is a 38/256 chance the game will go for a random 'hax item', namely one of
BrightPowder, King's Rock, Quick Claw, or Scope Lens.

Lastly the remaining 13/256 chance is for a special species-exclusive item:

* Pikachu: Light Ball
* Farfetch'd: Stick
* Cubone/Marowak: Thick Club
* Chansey: Lucky Punch
* Ditto: Metal Powder

If the Pokémon is not one of these species, the game instead goes for a hax item
as above.

After the held item is decided, the Pokémon's moves are shuffled.

## Stats and miscellany

DVs are generated randomly. Due to how Generation II works, this means Attack,
Defense, Special, and Speed DVs are each random numbers from 0 to 15, and HP is
determined by these. Shininess is determined by DVs, as is gender.

![Shiny Azumarill in Challenge Cup](/assets/img/ps2-shiny-azumarill.png)
_Shiny Pokémon have these sparkles, in case you think they're common_

Stat experience is set differently for the player and the AI. For the player,
all of a Pokémon's stats' stat experience is set to the same value, namely
(680 - Base Stat Total) * 65535 / 500 rounded down. For the AI, the same stat
experience spread is used for all of their Pokémon, and this is higher in
Round 2. Generally this is the same value for all stats, but a few trainers have
speed stat experience set differently.

| Trainer           | Division    | Round One Stat Experience | Round Two Stat Experience |
| ----------------- | ----------- | ------------------------- | ------------------------- |
| Camper Marcus     | Poké Ball   | 6400                      | 9000                      |
| Rocket ♂          | Poké Ball   | 12800                     | 25600                     |
| Picnicker Melissa | Poké Ball   | 6000                      | 36000                     |
| Guitarist Daren   | Poké Ball   | 12800                     | 28000                     |
| Fisherman Curtis  | Poké Ball   | 20000                     | 58000, but 30000 speed    |
| Medium Peggy      | Poké Ball   | 12800                     | 32000                     |
| Rocket ♀          | Poké Ball   | 20000, but 51200 speed    | 55000, but 65535 speed    |
| Juggler Dwight    | Poké Ball   | 36000                     | 65535                     |
| Twins Jan & Jane  | Great Ball  | 6400                      | 8000                      |
| Schoolboy Oliver  | Great Ball  | 11000                     | 30000                     |
| Sailor Curt       | Great Ball  | 25600                     | 35000                     |
| Swimmer♀ Darcy    | Great Ball  | 15000                     | 27000                     |
| Officer Gerald    | Great Ball  | 20000                     | 40000                     |
| Kimono Girl Emiko | Great Ball  | 25600                     | 50000                     |
| Scientist Roberto | Great Ball  | 22000                     | 45000                     |
| Gentleman Travis  | Great Ball  | 30000                     | 60000                     |
| Camper Marcus     | Ultra Ball  | 3200                      | 4500                      |
| Rocket ♂          | Ultra Ball  | 6400                      | 22000                     |
| Picnicker Melissa | Ultra Ball  | 14000                     | 35000                     |
| Guitarist Daren   | Ultra Ball  | 12000                     | 30000                     |
| Fisherman Curtis  | Ultra Ball  | 16000                     | 40000                     |
| Medium Peggy      | Ultra Ball  | 7500                      | 28000                     |
| Rocket ♀          | Ultra Ball  | 14000, but 40000 speed    | 35000, but 65535 speed    |
| Juggler Dwight    | Ultra Ball  | 25600                     | 50000                     |
| Twins Jan & Jane  | Master Ball | 500                       | 3200                      |
| Schoolboy Oliver  | Master Ball | 7500                      | 12800                     |
| Sailor Curt       | Master Ball | 12800                     | 20000                     |
| Swimmer♀ Darcy    | Master Ball | 7500                      | 18000                     |
| Officer Gerald    | Master Ball | 14000                     | 30000                     |
| Kimono Girl Emiko | Master Ball | 16000                     | 35000                     |
| Scientist Roberto | Master Ball | 12800                     | 33000                     |
| Gentleman Travis  | Master Ball | 22400                     | 47000                     |

It might seem strange that higher division opponents tend to have lower stat
experience, but bear in mind that the player will have less stat experience in
higher divisions due to its dependence on the base stat total.

Friendship is a random value from 0 to 255 inclusive. While Return and
Frustration are not possible to get in Challenge Cup, this can still come up due
to Metronome.

Lastly Pikachu has a 1/256 chance of being a [Talking Pikachu][talking-pikachu],
which makes more anime-like sounds and is a result of using a Pikachu from
Pokémon Yellow. Fun fact, a shiny Talking Pikachu is impossible in Challenge Cup
due to some weaknesses in the random number generation. But that rant is for
another post.

After all of this, the team members are shuffled and the team is complete.

[cc-sim]: https://genericmadscientist.github.io/Challenge-Cup-Sim
[cc-sim-repo]: https://github.com/GenericMadScientist/Challenge-Cup-Sim
[dbz-b3]: https://dragonball.fandom.com/wiki/Dragon_Ball_Z:_Budokai_3
[dbz-bt2]: https://dragonball.fandom.com/wiki/Dragon_Ball_Z:_Budokai_Tenkaichi_2
[stadium-2]: https://bulbapedia.bulbagarden.net/wiki/Pok%C3%A9mon_Stadium_2
[talking-pikachu]: https://www.youtube.com/watch?v=x2LsBRmVcEw

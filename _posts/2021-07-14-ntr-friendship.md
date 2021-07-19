---
title: "Friends in Yu-Gi-Oh! Nightmare Troubadour"
date: 2021-07-19 18:32:02 +0100
categories: [Yu-Gi-Oh! Nightmare Troubadour]
---
This is the third and final post I plan to do on
[Yu-Gi-Oh! Nightmare Troubadour][nightmare-troubadour]. I may come back to it at
a later date, but for now I've worked out everything I really wanted to know.
Like the previous two posts, this work was done with the PAL version of the
game. I did check the trades in the NTSC-U version as well just to make sure on
Imperial Order, though.

I'm not the first one to look into this aspect of the game. LancetJades on
GameFAQs has a topic [here][gamefaqs-post] where they dig into the same things
this post goes into. I independently found most of this, but their work on the
dislike counter saved me some time trying to make heads or tails of that part.

## Overview

As discussed in the [previous post][previous-post], Yu-Gi-Oh! Nightmare
Troubadour places the potential opponents on an overworld and the player must
track them down. However, each duelist also has a friendship level. If the
friendship level is high enough, the duelist will offer to register you. This
will allow you to immediately duel them if they have spawned, and be able to
tell who they are.

![Some registered duelists](/assets/img/ntr-registered-duelists.png)
_The jewel indicates Odion and Marik are registered_

Registering a duelist brings other benefits. It opens up the possibility for
them to give you their deck recipe (a listing of the cards contained in one of
their decks) and to trade with them. Neglect a duelist too long though, and they
will deregister you.

## Making friends

Friendship is given by a non-negative two-byte value for each duelist. The game
divides friendship into five grades, given by emoticons with different
background colours. These five grades are what the player can view in-game.

Every duelist has their own values of friendship needed for each grade.
Additionally, each duelist has a maximum friendship and if the maximum
friendship is not greater than a threshold (note: greater, not greater than or
equal) then the game will not allow the duelist to reach that grade. This is how
the game keeps some duelists at the lowest grade permanently: Arkana, Bandit
Keith, Big 1, Big 2, Big 3, Big 4, Big 5, Gozaburo, Lumis, Noah, PaniK, Rare
Hunter, Umbra, Yami Bakura, and Yami Marik. This is also how the game stops some
duelists from reaching the maximum grade, namely Bonz, Dox, Ishizu Ishtar, Para,
Seto Kaiba, Solomon, Strings, and Weevil.

With that said, the table below contains the thresholds for each grade for the
duelists not permanently stuck at zero friendship. Note for some duelists lower
thresholds are given as -, which means the duelist can not drop below the
corresponding grade. This table also includes the initial friendship of each
duelist (set when you first encounter them), the maximum friendship, and a 'base
increase' value.

| Name          | :(  | :\| | :)   | X)   | Initial | Maximum | Base increase |
| ------------- | --- | --- | ---- | ---- | ------- | ------- | ------------- |
| Yami Yugi     |   - | 200 | 1100 | 1400 |     450 |    1600 |            20 |
| Yugi Muto     |   - | 200 | 1100 | 1400 |     450 |    1600 |            40 |
| Joey Wheeler  | 100 | 400 | 1100 | 1400 |     550 |    1600 |            16 |
| Seto Kaiba    | 200 | 600 | 1400 |    - |     350 |    1600 |            20 |
| Mokuba        |   - | 400 | 1200 | 1500 |     550 |    1800 |            30 |
| Bakura        |   - | 300 | 1200 | 1400 |     450 |    1500 |            22 |
| Tea Gardner   |   - | 200 | 1000 | 1300 |     450 |    1400 |            26 |
| Mai Valentine | 200 | 400 | 1100 | 1300 |     250 |    1400 |            12 |
| Serenity      |   - | 200 | 1000 | 1200 |     550 |    1300 |            26 |
| Rebecca       | 100 | 300 | 1100 | 1300 |     350 |    1500 |            16 |
| Solomon       |   - | 200 | 1200 |    - |     650 |    1400 |            24 |
| Bonz          |   - | 200 | 1200 |    - |     450 |    1400 |            18 |
| Mako          |   - | 300 | 1100 | 1400 |     150 |    1600 |            36 |
| Espa Roba     | 100 | 400 | 1100 | 1400 |     250 |    1600 |            14 |
| Rex Raptor    | 200 | 500 | 1400 | 1600 |     250 |    1800 |            14 |
| Weevil        | 300 | 900 | 1600 |    - |     550 |    1800 |            18 |
| Dox           | 100 | 400 | 1300 |    - |      50 |    1500 |            26 |
| Para          | 100 | 400 | 1300 |    - |      50 |    1500 |            26 |
| Pegasus       |   - | 200 |  900 | 1100 |      50 |    1300 |            26 |
| Strings       |   - | 200 | 1200 |    - |      50 |    1400 |            26 |
| Odion         |   - |   - |  800 | 1100 |     500 |    1300 |            16 |
| Ishizu Ishtar |   - | 200 | 1100 |    - |     450 |    1400 |            26 |
| Marik Ishtar  |   - |   - |  900 | 1200 |     200 |    1500 |            26 |

The base increase value plays a role in how friendship increases. When you enter
a duel against an opponent, the game increases their friendship (note that since
this happens upon entering a duel, whether you win or lose doesn't matter). The
increase is the sum of three numbers. The first is the duelist's base increase.
The second is the player's level. The third is 50 if this is the player's first
duel against that opponent that day, 25 if it's the second that day, and 0 for
subsequent duels.

There is a second way to increase friendship: accepting trades. Each duelist has
a value that is meant to give how much accepting a trade increases their
friendship by. I have not given these values because they do not matter since
everyone in this game is a complete nutter.

![Serenity offering a trade](/assets/img/ntr-serenity-trade.png)
_Accept the trade and she will be willing to die for you_

Presumably the code was meant to look something like this:

```c
increaseFriendship(opponentId, FriendshipData[opponentId - 1].increaseForTrade);
```

Instead it looks like this:

```c
increaseFriendship(opponentId,
                   friendshipLevel(opponentId) +
                       FriendshipData[opponentId - 1].increaseForTrade);
```

This means accepting a trade would at least double the duelist's friendship. But
since their friendship must be at least :) grade in order to offer a trade (more
on that later) and the threshold for :) grade is more than half the maximum
friendship for each duelist, it just maximises it instead.

After a duel, if the duelist's friendship is at least of :) grade, the game
checks if a duelist is registered. If not, the game does a roll. If the
friendship is of :) grade, there is a 60% chance the duelist will offer to
register you. If the friendship is of X) grade, the duelist will always offer to
register you.

## Losing friends

Losing friendship is more complicated. There are three ways to decrease a
duelist's friendship: rejecting a duel offer, rejecting a trade offer, and going
a day without dueling them. But it's not as simple as the friendship going down
by some set value every time one of these happens. Each duelist has a 'dislike
counter' that acts as a buffer stopping them from immediately losing friendship.
Relevant values are given in the table below.

| Name          | Threshold | End of day | Duel decline | Trade decline |
| ------------- | --------- | ---------- | ------------ | ------------- |
| Yami Yugi     |       180 |         10 |            4 |             2 |
| Yugi Muto     |       180 |         10 |            4 |             2 |
| Joey Wheeler  |       150 |         12 |            5 |             3 |
| Seto Kaiba    |       130 |         12 |            5 |             3 |
| Mokuba        |       120 |         12 |            5 |             3 |
| Bakura        |       130 |         16 |            7 |             5 |
| Tea Gardner   |       160 |         12 |            5 |             3 |
| Mai Valentine |       150 |         12 |            5 |             3 |
| Serenity      |       130 |         10 |            4 |             2 |
| Rebecca       |       130 |         14 |            6 |             3 |
| Solomon       |       180 |         10 |            4 |             2 |
| Bonz          |       150 |         12 |            5 |             3 |
| Mako          |       180 |         10 |            4 |             2 |
| Espa Roba     |       130 |         12 |            5 |             3 |
| Rex Raptor    |       130 |         14 |            6 |             3 |
| Weevil        |       130 |         14 |            6 |             3 |
| Dox           |       130 |         14 |            6 |             3 |
| Para          |       130 |         14 |            6 |             3 |
| Pegasus       |       150 |         12 |            5 |             3 |
| Strings       |       180 |         12 |            5 |             3 |
| Odion         |       180 |         10 |            4 |             2 |
| Ishizu Ishtar |       160 |         12 |            5 |             3 |
| Marik Ishtar  |       150 |         10 |            4 |             2 |

The dislike counter is reset whenever a player starts a duel against the duelist
in question. When one of the three above actions occur, the game looks up the
value for the given action and duelist. If the duelist is registered, this value
is halved. If the value was odd, the game rounds down. Since trades are only
offered to the player if the duelist is registered, this means that declining a
trade always sets the dislike counter to one, except for Bakura for whom it gets
set to two.

Next the game looks at the dislike counter and compares it to a per-duelist
threshold. If the dislike counter meets or exceeds this threshold, the value is
subtracted from the friendship. Otherwise the game first adds the value to the
dislike counter. If after this addition the dislike counter meets or exceeds the
threshold, the new dislike counter value plus the value are subtracted from the
friendship and the duelist will deregister the player. Note this means that the
final addition to the dislike counter is effectively taken off the friendship
twice.

## Deck recipes and trading

If the player has a duelist registered and the friendship is of :) grade or
greater, there is a chance the duelist will give you their deck recipe or
propose a trade. If the friendship is of :) grade, there is a 30% chance of this
happening. If the friendship is of X) grade, there is a 40% chance. Note that
this means it is more difficult to get the deck recipe from and trade with
duelists who max out at :) grade.

If the game decides to do one of these, it first checks if the duelist can give
out their deck recipe. This is the case provided the player does not already
have the deck recipe, and in three cases (Yami Yugi, Seto Kaiba, Marik Ishtar)
also whether they have the corresponding god card (Slifer, Obelisk, Ra). It does
not matter if they used a deck with the god card in it. Provided these checks
pass, there is a 50% chance the game will award the player with the deck recipe.
If this 50% fails, or the checks do not pass, the game will instead try to
propose a trade.

It is important to note there are two types of trades: random trades and fixed
trades. Each opponent has a pool of cards they would like and ones they are
willing to part with. In a random trade, the game selects a random card the
opponent is willing to part with, and asks for a random card they'd like out of
the ones the player is able to trade away (i.e. has at least one copy not
currently in the deck, fusion deck, and probably side deck).

Fixed trades are simpler: the opponent will ask for a specific card and offer a
specific card in return. Such a trade can only be proposed in the postgame after
Yami Marik has been defeated, if the player has a copy they can trade away, and
in the case where a god card is offered by the opponent, the opponent has said
god card.

If the AI has decided to try to propose a trade, it will look at the random
trade cards the player can trade away and the fixed trades that are possible. If
there are no trades of either type, the game abandons the attempt. If there are
only available trades of one type, the game will offer that type. If there are
available trades of both types, the game has a 50% chance of offering either
type of trade.

When the game decides to do a random trade, it will randomly select one of the
cards the player is able to give away (each with equal probability, ignoring
imperfections in the RNG and its utilisation), randomly select one of the cards
the opponent is willing to part with, and then offer to trade those two cards.
When the game decides to do a fixed trade, it takes the list of fixed trades
that are allowed and randomly selects one.

The list of cards each opponent will randomly request and offer, and their fixed
trades, have been placed in an XML file [here][trades]. The trades are the same
in the NTSC-U version.

Note that Imperial Order is a postgame trade, so it is impossible to get it from
Pegasus in the NTSC-U version.

[gamefaqs-post]: https://gamefaqs.gamespot.com/boards/920752-yu-gi-oh-nightmare-troubadour/78369242
[nightmare-troubadour]: https://yugipedia.com/wiki/Yu-Gi-Oh!_Nightmare_Troubadour
[previous-post]: {% post_url 2021-07-08-ntr-npc-spawns %}
[trades]: {% link assets/data/ntr-trades.xml %}

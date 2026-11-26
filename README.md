# Star Evolution: web test

A small, free, mobile-friendly test of **Star Evolution**, a risk-and-reward strategy wargame (the full game is a modular Tabletop Simulator wargame). This web version is a trimmed slice for testing the core ideas in 5 to 30 minutes, no install and no account.

**It is a test, not the finished game.** Every number is a placeholder, the concept matters more than the final balance, and your feedback is what fixes the oversights.

**Play:** https://evaunit34.github.io/star-evolution-web/ (open it on a phone or a computer)

## The idea

Three units a side on a hex map. Each turn every unit gets one move and one action. You score **star points** by claiming land, defeating enemies, opening chests, talking to the old hermit or the wandering god, and beating bosses. First side to the point goal wins, or the higher score when the clock or round limit ends.

Three factions form a triangle: **Star Warriors** (tanky, heal) beat **Chaos Warriors** (glass cannons, pierce) beat **Cultists** (attrition, hazards) beat **Star Warriors**. Each hit gets +2 damage against the faction you counter and -2 against the one that counters you.

## Three modes, each adds a layer

| Mode | Adds | Unlocks after |
|---|---|---|
| **Basic** | Moving, techniques and chi, claiming land, chests, the hermit, star points | Open from the start |
| **Standard** | Magic cards, karma and Soul Purge, the Wandering God, speed as movement range, Negate | 5 finished Basic matches |
| **Expert** | Cultist hazards, Speed Traps and Reversals (everyone), faction barrier flames, group attacks, an angry god you can calm, weaker healing and returns, optional lives | 10 finished Standard matches |

Testers can unlock everything in **Options > Unlock all modes**. In Custom Match every Expert rule can be switched on or off, so you can test one at a time.

## How to play

- **Touch or mouse:** tap a unit, tap a glowing tile to move, tap an enemy to preview and confirm an attack. Buttons handle Claim, Open chest, Talk and End turn.
- **Keyboard:** Q W E A S D move a cursor on the hex grid, Space confirms, Tab next unit, 1 and 2 pick a technique, C claim, V open chest, T talk, N end turn, Esc cancel.
- First time? A short picture tutorial and a guided first turn are offered. Both can be replayed from Options.
- **Quick Play** picks everything for you. **Custom Match** sets the rules, goal, clock, round limit and best of 3. **Practice Mode** lets you pick any faction against any other.

## Expert mode at a glance

- **Hazards (Cultists):** hidden tiles that drain life or chi from enemies next to them. Found by scanning, removed by dismantling.
- **Speed Traps and Reversals (everyone):** a unit hides one per turn. A Speed Trap slows enemies by 1; a Reversal throws an enemy hazard or trap back at the unit that set it.
- **Barriers:** a flame in each faction's colour. Blue (Star Warriors) soaks a limited number of pierce and hazard effects, red (Chaos) adds HP and cheaper self-damage, silver (Cultists) reflects some damage.
- **Group attacks:** twice per game, all three units attack together for a share of every enemy's max HP, at the cost of every action and 4 primary chi next turn.
- **Angry god:** anger rises when nobody talks to it, up to 5, and it smites the leader. Each talk lowers it by 1, or pay 15 points once per game to lower it by 3.
- **Weaker returns:** defeated units return with 80%, then 70%, then 50% of max HP.

## Feedback

Please tell us what felt confusing, unfair, fun or boring: which faction you played, which mode, and roughly what happened. Open an Issue on this repo or message the maker directly. Balance numbers are expected to change.

## For modders

The game is one self-contained file. All numbers live in plain data tables near the top of the page's scripts, not buried in the logic. Open `index.html` in a text editor and search for:

- `SE.data.rules` for points, costs, limits and the faction edge
- `SE.data.standard` for karma, Soul Purge, the god and card limits
- `SE.data.expert` for hazards, traps, the god meter, barriers, group attacks and revive HP
- `SE.data.units` and `SE.data.techniques` for units and attacks

Change a number, save, reload. CPU opponents are simple priority lists with a little randomness, so they are a test partner rather than a challenge.

## Status

A work in progress. Planned later: Faction Builder (swap units and card builds), key rebinding, a free Play Store app, and longer modes. Full design notes and a data workbook are kept separately.

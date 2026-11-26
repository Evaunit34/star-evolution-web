# Star Evolution: web test

A small, free, mobile-friendly test of **Star Evolution**, a risk-and-reward strategy wargame (the full game is a modular Tabletop Simulator wargame). This web version is a trimmed slice for testing the core ideas in 5 to 30 minutes, no install and no account.

**It is a test, not the finished game.** Every number is a placeholder, the concept matters more than the final balance, and your feedback is what fixes the oversights.

**Play:** https://evaunit34.github.io/star-evolution-web/ (open it on a phone or a computer)

## The idea

Three units a side on a hex map (9 by 7, or 11 by 9 and random in Standard). Each turn every unit gets one move and one action. You score **star points** by claiming land (10), defeating enemies (1), opening chests (worth their level, and chests grow from level 1 to 5 as the match goes on, +1 every 4 rounds), talking to the old hermit or the wandering god, and beating bosses. First side to the point goal wins, or the higher score when the clock or round limit ends. Defeated units return at their base with full HP next turn, so a fight costs tempo, not the match. Every unit's second attack has 2 less range than before (never below 2), and the free grey attack always does at least half its power, so it stays a real fallback.

Three factions form a triangle: **Star Warriors** (tanky, heal) beat **Chaos Warriors** (glass cannons, pierce) beat **Cultists** (attrition, hazards) beat **Star Warriors**. Each hit gets +2 damage against the faction you counter and -2 against the one that counters you.

## Three modes, each adds a layer

| Mode | Adds | Unlocks after |
|---|---|---|
| **Intro** | Moving, techniques and chi, claiming land, chests, the hermit, star points | Open from the start |
| **Basic** | Magic cards (damage spells are aimed at a 2x2 area; hover a card to read it), karma and Soul Purge, the Wandering God, speed as movement range, Negate | 5 finished Intro matches |
| **Standard** | Cultist hazards, Speed Traps and Reversals (everyone), faction barrier flames, group attacks, an angry god you can calm, weaker healing, a bigger random map, optional lives | 10 finished Basic matches |

Testers can unlock everything in **Options > Unlock all modes**. In Custom Match every Standard rule can be switched on or off, so you can test one at a time.

## How to play

- **Touch or mouse:** tap a unit, tap a glowing tile to move, tap an enemy to preview and confirm an attack. Buttons handle Claim, Open chest, Talk and End turn.
- **Keyboard:** Q W E A S D move a cursor on the hex grid, Space confirms, Tab next unit, 1 and 2 pick a technique, C claim, V open chest, T talk, N end turn, Esc cancel.
- First time? A short picture tutorial and a guided first turn are offered, and the first Basic and Standard matches show a step-by-step guide banner. The Guide has picture cards for Basic and Standard too. All can be replayed from Options.
- **Two players, one device:** in Custom Match set Opponent to the second player. A pass-the-device screen hides the board between turns, so it also works over screen sharing or Parsec.
- **Preference Play:** turn it on in Options for three saved match setups on the main menu (faction can be fixed or asked each time).
- **Quick Play** picks everything for you. **Custom Match** sets the rules, goal, clock, round limit and best of 3. **Practice Mode** lets you pick any faction against any other.

## Standard mode at a glance

- **Hazards (Cultists):** hidden tiles that drain life or chi from enemies next to them. Found by scanning, removed by dismantling.
- **Speed Traps and Reversals (everyone):** a unit hides one per turn. A Speed Trap slows enemies by 1; a Reversal throws an enemy hazard or trap back at the unit that set it.
- **Barriers:** a flame in each faction's colour. Blue (Star Warriors) adds +12 max HP and soaks a limited number of pierce and hazard effects, red (Chaos) adds HP and cheaper self-damage, silver (Cultists) reflects some damage.
- **Group attacks:** twice per game, all three units attack together for a share of every enemy's max HP, at the cost of every action and 4 primary chi next turn.
- **Angry god:** anger rises when nobody talks to it, up to 5, and it smites the leader. Each talk lowers it by 1, or pay 15 points once per game to lower it by 3.
- **Bigger random map:** an 11 by 9 board with land, chests, water and bosses placed at random each match, always in mirrored pairs so both sides are equal. It forces you to move around.

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

## Modes and CPU (latest pass)
- **Chaos** (opts.chaos): every 2 rounds a random event (quake, new chest, chi surge, storm on the leader). `js/modes.js`, numbers in `SE.data.modes`.
- **Fog of war** (opts.fog): enemies beyond 3 tiles are hidden from you; **Investigate** (uses the unit's action) reveals all until turn end. The CPU is not affected by fog.
- **EX** (opts.ex, Standard only): one extra god smite once the god is angry; clock 25% shorter.
- **Hard CPU** (opts.cpu "hard"; sims: `HARD=A|B`, `HC=threat,threatLow,jitter,focus,land,chest,lethal`): focus fire, killers act first, lower jitter. Sims show only about +1-2% over Normal, so Hard is a modest bump for now.
- Hover (desktop) or tap (mobile, nothing selected) an enemy to see its stats. Options and Custom Match are now collapsible groups; new toggles: screen shake, god-anger pulse, neck-and-neck glow.
- Tests: `node tests/modes.test.js`; sims accept `CHAOS=1 FOG=1 EX=1 EXPERT=1`.

- Cue toggles: Options > Effects (shake and red flash on big hits, god-anger pulse, neck-and-neck glow) and Options > Sound (sounds for those cues).
- **Defenders** (opts.defender; sims `DEF=1`): one per side, roams on or next to its owner's claimed land, hits enemy units within 2 tiles at the start of their turn for 2 x level (never below 1 HP). HP 10 +2 per level (max 18), DEF 5, level 1 to 5 by round (+1 every 2 rounds), keeps its level on death. Killed only by area spells, group attacks (80% of max HP) and hazards (double damage); no points; needs a turn to return near claimed land. Numbers in `SE.data.modes.defender`. Sims: matches run about 1 to 2 rounds longer, win rates unchanged.
- Hit feedback: no screen shake; the unit that is hit shakes harder, and critical hits (15+ damage) shake it much more and flash the map red.

## Latest changes
- **Mode names:** Intro (engine id `basic`), Basic (`standard`), Standard (`expert`). Only the display text changed.
- **Tutorials menu** (Options > Tutorials): Intro how to play, Intro guided turn, Basic and Standard guided first turns (each starts a real match with the banner), and a reset button.
- **Red screen flash** on critical hits is now its own toggle (Options > Effects and tension), off by default.
- **Faction Builder** (`js/builder.js`, main menu): pick 3 of a faction's 5 units (first pick is the commander) and a magic deck: 12 cards (ranks 3/2/2/2/3) or 20 (5/5/4/3/3), up to 2 copies, 5 builds per faction, saved in the browser. Choose "Your build" in Custom Match (or Play from the builder). CPU keeps default units and deck. Custom units are shown as coming later. Tests in `tests/modes.test.js`.

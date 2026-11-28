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
- **Guided turns now lock input** (Basic and Standard): only the current step's buttons (and card plays) work; selecting, moving, attacking and reading cards always work, the god step also allows End turn, and Skip guide unlocks everything. Shown the first time each mode is played; replay from Options > Tutorials.

## The Magician (Standard mode god)
In Standard mode (engine "expert") the god is The Magician, shown with her card art. Once angry she uses one move a round (data/expert.js `god.byAnger`, `god.moves`): anger 2-3 Divine Magic Fist (10 dmg to the unit with the most HP on the leading side, +4 after 5 uses), anger 4 Divine Lightning (3x3 on the biggest cluster, 10 dmg, +1 every 2 rounds), anger 5 Bad Luck (-4 HP to every unit on both sides, never lethal). She can be attacked, and magic card area damage can hit her too, but each hit only raises her anger (spells never kill her) (HP 300, DEF 30). Killing her would give 35 points and +30 karma, but she is not reachable yet. Basic mode keeps the friendly Wandering God.

## Summon request (was "Visit")
Any mode: the Summon button ("Summon to talk") (free, no action) asks The Magician to come next to your closest unit. Once every 2 rounds per side. If the other side already asked this round, it is a coin flip (a lost flip does not use the cooldown). She still wanders after talks. The CPU asks when all its units are more than 2 tiles away and she is open to talking. Rules are in js/modes.js (R.canVisit, R.visit).

## Phone layout
Under 900px wide the panel is compact: a one-line unit summary and a bottom bar with Action / Talk / Cards / Group / Info and End turn. Tapping a tab opens that section above the bar; picking an attack, card or action closes it. Selecting a unit opens Action. Landscape phones put the map on the left and the panel on the right. Desktop keeps the full panel. Code: panelHtml and render in js/ui.js (sections have data-s, the bar has data-dock), CSS at the end of style.css.

## Minimal phone UI
On phones the top bar shows only the two factions with scores, Goal and Round; the ... button reveals the clock, turn, god meter and Menu. Technique buttons show name and cost only (Options > Button descriptions: Auto / Short / Full; Auto is Full on desktop). Picking an attack shows a tag on the closest visible enemy: average damage (minus the target's DEF), "KO" if it would defeat them, or "N too far" (Options > Attack tags). The long tip is behind the ? button.

## Collapsible bottom panel
The arrow button at the left of the bottom bar collapses the whole panel to the arrow, a short unit label and End turn (saved in the panelCollapsed setting). The attack preview (confirm), card target picking, guided turns and the end screen expand it automatically. In landscape the collapsed panel becomes a narrow right-hand strip, so the map gets the full width. Landscape also now fits the whole board on screen (the bottom rows were cut off before).

## Map first (phones)
Defaults: top info is a thin overlay on the map (Options > Layout: bar instead), the bottom panel is collapsed and does not open by itself (it still opens for guided turns), and everything that used the panel shows as a chip on the map: Attack / Cancel for the attack preview, Cancel while picking a card target, Move / Cancel for movement, and hints with OK. Pinch to zoom, drag to pan, Fit button to reset (mouse wheel zooms on desktop); the zoom window uses the spare screen height in portrait. Movement now needs a confirm: tap the tile, then tap it again or press Move (Options: Confirm moves; guided turns skip the confirm). Settings: topMode, panelCollapsed (null = auto), zoom, confirmMove.

## Unit chip and Special menu (phones, panel collapsed)
Tapping one of your units shows a chip on the map with what it can do where it stands (Claim with its HP cost, Open chest, Talk or Summon, Investigate under fog). Tap the unit again to hide the chip. After a move the chip shows by itself. In Standard mode the chip has a Special button that swaps it for that unit's specials: Barrier, Trap, Dismantle, Hazard (Cultists), the Group action, Soul Purge (commander), Calm god; unavailable ones are dimmed and tell you why when tapped. Code: SE.ui.unitChip in js/ui.js.

## Map info on demand
Unit data on the map (HP bar, level, barrier flame) can show only for the selected unit, the attack target, a hovered unit, or a tapped one. Setting `mapInfo`: `auto` (on demand on phones, always on desktop), `tap` (always on demand), `always`. Toggle in Options (Display) or the Info tab of the bottom panel. Land and chest numbers stay visible.

## Attack chip
The attack preview chip has a row of the unit's usable techniques; tap one to switch the preview.

## Enemy info strip
`mapInfo: "strip"` hides unit data on the map and shows a collapsible list of enemies (name, level, HP, chi) above the board, closest to your selected unit first; tap a row to select/attack that enemy. The strip's open/closed state is saved (`stripOpen`). Choose the mode in Options or the Info tab of the bottom panel (Auto / Tap-hover / Strip / Always). The strip floats at the top-left of the map in landscape too (not yet moved into the side panel).

## Tutorial and guides for phones
Tutorial cards now include "The phone screen" (top bar, chip, bottom bar tabs, pinch zoom) and "Reading the map" (data on tap, enemy strip). Move, Attack, Claim, Chest, Talk, hazard, trap and Group texts describe the unit chip and the Special menu. Hints, the guided lesson and the guided turns use the same wording. The guide's last tab is now "Controls": a Touch table plus the keyboard keys.

## Strip placement and tap info
With the strip on, it sits at the top of the side panel in landscape and on desktop (when the panel is open). In portrait, or when the landscape panel is collapsed (too narrow), it floats over the map. While the strip is on, tapping or hovering enemies and NPCs no longer pops their info; turn `stripTap` on (Options) to get both.

## Actions and Team sheet
Setting `actsUi` (Options). The unit chip's **Actions** button opens a sheet with the selected unit's options: Move, each technique (range, chi cost, targets in range), Claim, Open chest, Talk or Summon, Investigate, and the Standard Special actions. Unavailable rows are greyed with the reason; available rows start the action. The **Team** tab (bottom bar button with a "N left" count, or the panel row on desktop) lists every unit as Ready, Moved or Done with what it can do, and **Show all moves on map** tints every tile any ready unit can reach and ringing every enemy any unit can attack. It is information and shortcuts only: no new rules. Hidden in the guided lesson and guided turns.

## Area spell and Group attack preview
Aimed damage cards: tap a tile and every unit in the 2x2 area gets a tag with the damage it would take (after DEF), "KO" if it would be defeated, and your own units are tagged in blue "yours". The map chip adds a summary ("Hits 2 enemy units, 1 defeated, and 1 of yours") and a **Cast** button (tapping the same tile again still casts). The Magician is tagged "(anger)": magic raises her anger but never kills her. The Group attack no longer opens a full-screen popup: it puts a chip on the map with its cost and **Unleash** / **Cancel**, and tags every enemy with its damage. Preview code: `R.cardPreview` (js/standard.js) and `R.groupPreview` (js/expert.js). Not previewed: Negate responses.

## Turn pill
The top row shows "Your turn · moved N/3" (or "CPU turn", or the player's name in hot seat). When every unit has acted it turns green and says "All done: tap End turn" (this nudge follows the Hints setting; the counter is always shown).

## First-time pointers
With Hints on, a green arrow with a one-word label points at the first thing you have not done yet: capture land (nearest unclaimed land), attack (nearest enemy), open a chest, talk to The Magician, summon her, then in Standard play a magic card, set a trap, a hazard, use the Group attack (after you have claimed and attacked). Cards, Trap, Hazard, Group and Summon pulse the matching button instead of, or as well as, an arrow. Doing the thing once removes its pointer for good (saved in the `done` setting, across matches). One pointer at a time; an ignored one fades after 4 rounds in that match. The arrow hides while you preview an attack, pick a target or confirm a move.
Accessibility (Options): pointer style (Arrows / Arrows and text / Text only), larger arrows, keep arrows still (also still when the system asks to reduce motion or Attack animations are off). The arrow has a white outline and a shape, so it never relies on colour. "Show first-time pointers again" resets them. If a button target is not on screen (desktop with no dock), the text hint is shown instead. The older contextual hints also stop once the thing has been done.

## Info fades after a few seconds
Tapped unit info no longer hangs: the pop-up tip and the on-map data (HP bar, level) fade about 4 seconds after a tap, except for your selected unit and the attack target. On touch devices hover-style tips also time out. With a mouse, hovering still shows data for as long as you hover.

## Pointer on/off
`pointer` setting: Arrows, Arrows and text, Text only, or Off. The Info tab of the panel has a quick "First-time pointers" checkbox (on = Arrows, off = Off), and Options has the full list. The choice is saved and applies to every match until you change it.

## Keep showing pointers every match
Setting `pointerAlways` (Options: "Keep showing pointers every match"). Off (default): a pointer disappears for good once you have done the thing. On: "done" is tracked per match only, so the pointers walk through capture, attack, chest, talk and so on again in every match. Pointer style, size, still-arrows and the on/off checkbox work the same way in both.

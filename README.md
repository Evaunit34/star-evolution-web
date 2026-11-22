# Star Evolution - web test build (Pass A)

A trimmed, mobile-friendly slice of Star Evolution for quick tests (5-20 minutes). Not a port of the TTS game. All numbers and the two factions are **placeholders** until playtests and the real unit sheet.

## Run it
Double-click `index.html` (no server needed). To share it: put the `web/` folder on GitHub Pages (Settings > Pages > deploy from a branch, folder `/web` or copy it to `/docs`).

## What is in Pass A
Main menu (Faction Builder and Practice Mode are "coming soon"), Quick Play, Custom Match (Basic rules; star point goal, match clock, round limit, defeated units return, hints), a four-card skippable tutorial, hex board, a basic CPU, options (text size, colour-blind mode, hints, keyboard list), pause menu, result screen.

## Controls
Mouse/touch: tap a unit, tap a glowing tile to move, tap an enemy to attack (pick the technique first with the buttons), buttons for Claim / Open / Talk / End turn.
Keyboard: Q W E A S D move a cursor on the hex grid (up-left, up, up-right, down-left, down, down-right), Space confirms, Tab next unit, 1 / 2 technique, C claim, V open chest, T talk, N end turn, Esc cancel. Bindings live in `js/store.js` (`SE.defaultKeys`); a rebinding screen is Pass C.

## Rules (Basic)
3 units a side, one move + one action each per turn. Star points: capture land 10 (each claim, steals included), kill 1, chest 1 x level, mini boss 2 x level, talk to the NPC 5 (once per side). Claiming land costs 2 x level HP and is refused if it would defeat the unit; it also gives a level (once per tile per side). Wardens (mini bosses) hit everyone next to them at the end of each round. Defeated units return at their start tile next own turn at half HP (switchable in Custom Match; without it a wipe-out ends the match). First side to the point goal wins; on a clock or round limit the higher score wins (ties: land held, then total HP).

## Change the numbers
Everything lives in `data/*.js` (plain data objects): `rules.js` (points, costs, limits, defaults), `techniques.js`, `units.js` (factions and units, with a reserved `instincts` field), `map.js` (board, spawns, land, chests, NPC, bosses).

## Tests
`node tests/rules.test.js` (rules), `node tests/sim.js 400 100` (CPU vs CPU batch: games, points limit). Browser check used Playwright (see the session notes).

## Layout
`js/hex.js` hex maths, `js/rules.js` the engine (no screen code), `js/ai.js` basic CPU, `js/store.js` saved settings and key table, `js/ui.js` match screen, `js/menu.js` menus and flow.

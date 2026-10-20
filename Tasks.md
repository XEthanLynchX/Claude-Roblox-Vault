# Tasks

## In progress
- [ ] Set up the two-place experience (Lobby start place + Game place) and fill Config.Places.GamePlaceId
- [ ] Playtest the chore build (solo + 2 players)
- [ ] Replace built-in props with real assets (see [[Systems/Assets]])

## To do: first playable version (target: end of October)
- [x] Bigger wing map: connected hallways + a few enterable classrooms, custodian closet, exit door (randomized per wing)
- [x] Chore system (server): chore list per wing, scaled by player count; progress HUD / clipboard
- [x] First chore types: mop spill, empty trash, lock classroom
- [x] Wing key + exit door → next wing
- [x] Convert anomalies into private scares (reuse delivery system), add loud sound scares
- [x] Fake chores from low sanity
- [x] Closer: model imported, wanders, undoes chores, hunts, jumpscare
- [x] Closer menace-meter AI (phases, entry points + vents, hunt triggers, retreat, light flicker) and hideable lockers
- [ ] Playtest + tune menace numbers (arrival ~1.5–2 min on Wing 1?)
- [ ] Real sounds: vent hatch, door rattle, PA voice
- [ ] Upload new sounds (MusicBox, Drone, Tension, Chase, JumpScream, JumpHit, Tinnitus, Static) and paste ids in Config.Sounds
- [ ] Playtest the door peek (lean direction / head poking past the frame)
- [ ] Revive (Robux / ad, once per run) for taken players
- [ ] Spectate view for taken players
- [x] Remove the door vote / loop service
- [ ] Playtest: is splitting up + scares fun?

- [ ] Playtest the new chores with 2 players (ladder, phone, cameras) and tune their numbers
- [ ] Check held items in third person / first person and tune `Items.Defs` Hold/View

## Later
- [ ] Teleport players back to the lobby after a run (dawn or everyone taken)
- [ ] Host save slot chosen in the lobby
- [ ] Show carried items (trash bag, key) in first-person view
- [x] Upload the sound pack and fill in Config.Sounds
- [ ] Replace generated placeholder sounds with better ones where they fall short
- [ ] Hallway turns / second corridor for layout variety
- [ ] More chore types and wing themes (gym, cafeteria, basement...)
- [ ] Missing-janitor belongings (optional collectibles, lore)
- [ ] Time pressure tuning, revives, saves (3 slots), shop, cosmetics, journal, Graveyard Shift
- [ ] Lobby break room, Principal intro and voice lines

## Done
- [x] 3-slot hotbar inventory; office room; 10 new chores (2026-10-19)
- [x] Bigger wings (branch corridor), first person, sprint/stamina, flashlight batteries, blackouts + breaker, sanity monitor, fuller custodian room
- [x] Concept, setting and pre-development decisions ([[Design/Decisions]])
- [x] Rojo project + Git repo
- [x] First greybox loop (playtested; led to the objective pivot)
- [x] Private-delivery system, hidden sanity, flashlight, dark lighting

# Tasks

## In progress
- [ ] Playtest hiding with 2 players: locker player invisible to the other, desk pose looks right from outside, crawl in/out glides at the new 0.8 scale
- [ ] Check the smaller (0.8) characters everywhere: doors, ladder chore, first-person body, jumpscare framing
- [ ] Listen to the creepy voice in game and tune make_voices.py if it's too clean / too noisy
- [ ] Playtest the tutorial with 2 players and on touch/gamepad
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
- [ ] Spectate view for taken players: now designed as CCTV camera spectate + occasional scare option, see [[Design/After Death]] (design settled 2026-10-21, not built, wait for Ethan's go; revives are purchase-only)
- [ ] Always give a flashlight on respawn (revive, next wing, any respawn); see [[Design/After Death]] (not built, wait for go)
- [x] Remove the door vote / loop service
- [ ] Playtest: is splitting up + scares fun?

- [ ] Playtest the new chores with 2 players (ladder, phone, cameras) and tune their numbers
- [ ] Check held items in third person (broom/mop/plunger grips were fixed 2026-10-20; vacuum still carried oddly) and the tool arm animations with 2 players

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
- [x] Hiding fixes (locker invisible, desk knee-hug pose, no desk vignette), players at 0.8 scale, voice ids, the Closer on the CCTV feeds with a 35% leap at the lens (2026-10-20)
- [x] Every spoken line voiced: phone caller + private PA scare, normal in the PA voice, creepy in a glitchy voice (2026-10-20)
- [x] School backpacks replace the old ones: against walls, under desks, on coat hangers (2026-10-20)
- [x] Export Studio-only Schooler/Library scripts (+ Studio DevService) to src, commit `226f2d5` (2026-10-20)
- [x] First-time tutorial in Wing 1 (H replays, saved once per player); bigger journal with page-turn hints (2026-10-20)
- [x] Cleanup pass: gym exit doors, one thermostat, no flags, blackout kills vending glow, Schooler not in Wing 1, dev monster spawns, Closer arrives later (2026-10-20)
- [x] 17-point chore pass: horror prompts, physical mopping, tool animations, plunger skill check, thermostat/valve/breaker UIs, phone/clock/bulb/whiteboard/graffiti/camera fixes (2026-10-20)
- [x] 3-slot hotbar inventory; office room; 10 new chores (2026-10-19)
- [x] Bigger wings (branch corridor), first person, sprint/stamina, flashlight batteries, blackouts + breaker, sanity monitor, fuller custodian room
- [x] Concept, setting and pre-development decisions ([[Design/Decisions]])
- [x] Rojo project + Git repo
- [x] First greybox loop (playtested; led to the objective pivot)
- [x] Private-delivery system, hidden sanity, flashlight, dark lighting

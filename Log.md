# Log

Decisions and end-of-session recaps, newest first.

## 2026-10-20 (late: hiding fixes, smaller players, CCTV Closer)
- Voice ids found in Ethan's inventory (search_asset scope user): PAVoiceExtra 88268153417320, CreepyVoice 84267249437395, now in Config.Sounds.
- Lockers: player attribute `HidePose` = "Locker" once the door shuts; every other client hides that character (LocalTransparencyModifier) and its name.
- Desks: `HidePose` = "Desk" while sat under a desk. Every client poses that character hugging its knees (client Hiding, PreSimulation, Motor6D or AnimationConstraint `.Transform`). Ethan's avatar uses AnimationConstraints, not Motor6Ds. On these joints +X tips a limb BACKWARDS (checked in a playtest). Root sits at 0.35 x root height; the hider's camera offset is Config.Hiding.DeskCameraY (-0.6). No more black vignette; the hint fades after 3 s.
- Players scaled to Config.Characters.Scale = 0.8 (WingService `shrink`, Model:ScaleTo after the appearance loads). Hide glides use the real standing / sitting heights.
- CCTV: every figure is the Night Custodian (clone of ReplicatedStorage.CloserVisual + CloserSkin.attach). "Figure" events = walks across; "Closer" events (35%) = stares 1.15 s, then leaps into the lens (CloserSkin Strike; shriek, flash, shake, SIGNAL LOST). Don't anchor the clone's parts: Strike moves only the root.
- Found Studio-only edits in WingService, Movement, MusicPlayerService, MusicController (library / Schooler work) and exported them to src (per-line hash diff, then fetched only the differing lines).
- Commit `149fc4a`.
- Testing gotchas: requiring a service from execute_luau makes a second module instance, and Remotes.get then creates duplicate RemoteEvents (fire the original remote instead). user_keyboard_input didn't reach the game in this session (couldn't press E / C), so the real prompt-driven hide flow is untested; the hidden state, pose and camera were tested by setting them directly.

## 2026-10-20 (school backpacks, coat hangers, every line voiced)
- Ethan imported `school-backpack_roblox.glb`; set up as `Assets.SchoolBackpack`..`_5`, old Backpack..Backpack_6 moved to AssetBackups. His `coat hanger` model (was floating in Workspace) is now `Assets.CoatHanger`. Bags are placed against walls, under desks and on coat hangers. Commits `815ed0e`, `26e8a1c`. Details: [[Systems/Assets]].
- Ethan: every voice line must have audio; PA voice for normal lines, a creepier glitchy voice for creepy ones. The phone call and the private PA scare were text only. New `tools/make_voices.py`, packs `PAVoiceExtra` + `CreepyVoice`, `Shared/Voice.luau`. Commit `423ecf5`. Details: [[Systems/Voice]].
- **Ethan must upload** `sounds/PAVoiceExtra.ogg` and `sounds/CreepyVoice.ogg` and the ids go in `Config.Sounds.PAVoiceExtra` / `CreepyVoice` (silent until then, no errors).
- Studio restarted twice this session (studio_id changed each time); everything had been saved.
- Known console noise: asset 6160151677 "not authorized" = the backup Classic Radio in ServerStorage.AssetBackups (unused).
- Delete permission granted for the project + vault folders this session; stale git .lock files cleaned out.

## 2026-10-20 (Schooler/Library scripts into Git)
- Ethan: "Add to git". Exported SchoolerService, LibraryBuilder, LibraryService, SchoolerFx, LibraryFx and Studio's DevService to `src` (written from `execute_luau` output, then hash + length checked against Studio for each file: all six matched). Commit `226f2d5`.
- Method that worked: return `HASH path hash len` lines plus the sources from `execute_luau`, write each with a quoted heredoc in `device_bash`, compare the `x=(x*31+byte)%2147483647` hash in python. Studio source ends with a trailing newline (keep it).
- `git commit -- <paths>` fails for untracked files: `git add -- <paths>` and `git commit -- <paths>` in the same command, then move the leftover `.lock` files aside (HEAD.lock, next-index-*.lock, objects/maintenance.lock).
- Session setup: Studio MCP connected (studio_id from `list_roblox_studios`); Blender MCP tools load but Blender must be running with its add-on server (port 9876) first.

## 2026-10-20 (first-time tutorial)
- New TutorialService (server) + TutorialController (client). Plays once, the first time a player is in Wing 1; finishing or skipping saves `TutorialDone` in DataStore "PlayerFlags_v1" (key u_<UserId>). [H] replays it any time. In Studio it needs Game Settings > Security > Studio API access, otherwise it shows every Studio session.
- 11 short cards (8-year-old wording) at the top of the screen: welcome, flashlight (press F twice), batteries (ring on the meter, charger glows, R), journal (Tab), turn a page, close journal, chores (tools + custodian sign glow, E), next wing (exit doors glow), sanity x2 (sanity board glows), ready (H). Try-it cards wait for the action; others have NEXT / Enter. "Skip tutorial" bottom-left.
- While anyone is in the tutorial (ReplicatedStorage.TutorialActive) the school pauses: no menace, no Closer arrival, no blackout start, no scares. Flashlight battery doesn't drain during it and is topped up on the first run.
- Journal: held closer so it fills most of the screen; hotbar and held item hide while it's open; bottom-right key list (Click/→ next, Right-click/← back, Tab close) above the battery meter.

## 2026-10-20 (Closer arrives later)
- Ethan: the Night Custodian came in too fast. First arrival now needs menace >= 40 (was 30) AND at least `Closer.ArriveAfterSeconds` into the wing: 150 s on Wing 1, blending to 75 s on Wing 8. Dev spawn and the key hunt still bring it in immediately.

## 2026-10-20 (cleanup pass)
- Gym exit doors: no chains or padlock any more. Flags removed from rooms. Only one thermostat per wing (beside the first classroom door).
- Blackout now also kills "powered props" (LightFx powerProps): lights, emissive SurfaceAppearances and neon parts in the wing outside the Lights folder go dark and come back with the power. The exit sign is exempt (emergency battery). Covers the vending machines (VendingMachine_3 asset: SpotLight, SurfaceLight, emissive Body/Snacks/Int).
- Graffiti: spots are rejected if anything visible stands in front of the tag; hall tags sit at 9.4 high; no black paint colour.
- Schooler: never spawns in Wing 1 (SchoolerService.NewWing). Dev menu has "Spawn the Closer" / "Spawn the Schooler" (new SchoolerService.DevSpawn + DevService schooler_spawn).
- Note: SchoolerService, LibraryService and the other Schooler/library scripts exist only in Studio, not in src. Commit `32dd8b5`.

## 2026-10-20 (gym doors)
- The chained gym doors at the end of the back hall now use Assets.ExitDoors (same model as the wing exit), welded to the invisible GymDoors part so the Closer's entry rattle still shakes them; two crossed chains + a brass padlock in front of the push bars.

## 2026-10-20 (evening: bug pass + glossy look)
- End walls of the main and back halls now carry the lower paint band, chair rail and baseboard (they were bare plaster).
- Hallway clutter keeps 5 studs apart per side (wet floor signs were spawning inside each other).
- Removed two leftovers sitting at the hall start in Workspace: a copy of the Schooler and "Closer New Model" (the A-frame "ladder"). Both moved to ServerStorage.AssetBackups; the backup Schooler is renamed so SchoolerService's name search can't pick it.
- Bathroom graffiti moved to the door wall facing into the room (it was over the mirrors). Graffiti task: remaining paint tints every dirty cell, cells under 0.3 count as clean.
- Glossy look: generated MaterialVariants GlossyVinylTile1 (CeramicTiles, 8 studs/tile, diagonal tile grid) and GlossyPaintedPlaster (Plaster, 16 studs/tile), applied everywhere with MaterialService:SetBaseMaterialOverride (no code). Lighting.EnvironmentSpecularScale 0.2 -> 1, EnvironmentDiffuseScale 0.15 -> 0.25. Lives in the place file (save it). Commit `a9c3a97`.

## 2026-10-20 (afternoon: 17-point chore pass)
- Every interact prompt is now a custom horror plate (PromptUi): steel key cap, typewriter text, blood fill while holding. Drawn on a ScreenGui (BillboardGuis didn't show up).
- Mop a spill is physical: carry the mop (custodian room), hold left click by the puddle ~7 s (server-timed, puddle shrinks).
- Broom/vacuum/plunger/mop animate (ToolAnim). Root cause of the bad grips: Items.make set a PrimaryPart, which threw away the asset pivot; long tools now keep it (`KeepPivot`). Drops snap to the floor.
- Plunger: side skill check (3-5 hits), world stays visible. Thermostat: click up from cold to 68. Valve and breaker-box wires redrawn. Rewire swaps in the old breaker box asset.
- Phone: last question creepy only 25%. School name now on a plaque by the Principal's notice ("DAWNRIDGE HIGH SCHOOL, Est. 1962") and the notice is signed "Principal, Dawnridge High".
- Clocks name their rooms; dead-light prompts only show when you look at the fixture; "hold the ladder" taken out of the mix (code kept).
- Fixes: whiteboard said DONE early; graffiti was 1 stud inside the bathroom wall; camera feed was inside the camera housing (new SecurityCamera asset, pivot at the lens).
- Commit `75fc4b4`. Details: [[Systems/Chores]], [[Systems/UI]], [[Systems/Inventory]].

## 2026-10-20 (night: the Night Custodian is the Closer)
- Imported the Night Custodian into Studio (3D Importer, PBR textures + eye emissive) and made it the Closer. Old model backed up.
- Blender clips played in code (CloserAnims + CloserSkin). Low-sanity players (<25) see it crawl. New face jumpscare. Creepy voice + chase scream.
- Removed Workspace.VendingMachine_2 (moved to ServerStorage.AssetBackups as "VendingMachine_2 (removed from game)"). It was never an Assets variant, so wings never spawned it.
- Exit doors: imported `school_exit_doors_roblox.glb` (double door) as Assets.ExitDoors; replaces the flat exit door in every wing. See [[Systems/Assets]].
- Tested in play: idle, walk, run, crawl, jumpscare framing. Commit `61026a0`. Details: [[Design/The Closer]].

## 2026-10-19 (late evening: Night Custodian made realistic + Roblox-ready)
- Removed the mop. Re-sculpted the face (deep sockets, nasal cavity, gaping grin with crowded, broken teeth, glowing eyes), raised the cap so the face shows.
- New realistic materials (veins, cracks, pores, grime, dried/wet blood, fabric weave, leather), baked to 2048 maps and saved as 1024 PBR sets for Roblox.
- Game export = 4 meshes under 20k tris each + rig + 4 animations in `models/NightCustodian.fbx`. Details: [[Systems/Night Custodian Model]].

## 2026-10-19 (evening: Night Custodian 3D model)
- Rebuilt the Night Custodian concept sheet in Blender: skull-grin head, cap, torn bloody jumpsuit with AGNES patch, exposed spine spikes, claws, boots, mop.
- Rigged it (54 bones) and animated Walk, Run, Crawl and CrawlRun loops. Saved `models/NightCustodian.blend` + `.fbx`. Details: [[Systems/Night Custodian Model]].
- Blender is back in use for this model (Claude Design is still the default for 2D work). Textures are procedural, so they need baking before Roblox.

## 2026-10-19 (late afternoon: scarier, more professional UI)
- New `UiTheme` module and a single look for all screens; see [[Systems/UI]].
- Task screens restyled as dark steel "work orders" (red tab, W.O. number, rivets, grain, light flicker, dimmed world behind), amber-outlined buttons, a red DONE stamp on success, red flash + toast on failure. Screens scale to the screen size.
- Stamina is now a thin "breath" line that hides when rested; battery is a 5-cell battery icon with spare pips, low/DEAD states, and dims while the flashlight is put away. Hotbar restyled to match.
- Found while testing: the keyboard-input tool's leftover W key closes a task opened right after it (only affects testing).

## 2026-10-19 (afternoon: hotbar + 10 new chores)
- **Saved the night session's Studio-only code to src** first (commit `3d1e781`). Git delete permission is now granted for the project folder, so no more stale `.lock` files (old ones cleaned up).
- **Hotbar inventory** (3 slots, G drops, flashlight takes a slot and only shines while held). Trash bags and the wing key are hotbar items. See [[Systems/Inventory]].
- **New Office room** in every wing (WingBuilder slot 4, sign "MAIN OFFICE"): front desk with the PA phone, master clock (shift time), security desk with two CRT monitors, flashlight charging cabinet, bookshelf, posters ("NIGHT STAFF: ANSWER THE PHONE. ALWAYS.").
- **New chores** (ChoreService, mixed 2 newer : 1 classic):
  - Replace dead hall lights: spare tubes in a box in the custodian room, carry them (hotbar) to the dead fixtures.
  - Co-op ladder (only with 2+ players): a classroom light flickers; one holds the ladder, one climbs and swaps the tube (twist mini-game). If the holder lets go / is taken, the climber falls: ladder crash, menace +25, sanity -6, stunned 2.5 s. Last player alive may climb unheld.
  - Vacuum or sweep a hall: 6 dirt patches; vacuum = 0.6 s each but loud (menace per patch + per second running), push broom = 2.4 s each, quiet.
  - Restock the flashlight charger: carry 2 (solo) / 3 battery boxes from the custodian room to the office; then it hands out 2 spare batteries per player.
  - Watch the security cameras: 24 s grainy feed cycling 3 hall cameras; REPORT (Space) every movement (fake figures and a tall Closer-like one that stares into the lens, sanity -4). Missing one or 2 false reports fails.
  - Scrub graffiti: tags on bare hall walls, bathroom rear walls, the back hall end wall. At low sanity (<35) the words become your username or a teammate's (client-side).
  - Answer the PA phone (rings 12-35 s in, ring is noise): 3 questions in a row, 2 normal + 1 creepy; answers come from the Principal's notice, janitor notes, posters, wing number, staff count, chores left. 30% (never first) of calls are just breathing: hang up within 4 s or it hears you (menace +15, sanity -6). Fail = rings again in 10 s. Questions: `PhoneQuestions.luau`.
  - Put the chairs up (a classroom with chairs down, 6+ chairs): E on each chair.
  - Unclog a toilet: a stall is opened wide with a puddle in front; needs the plunger; timing mini-game, 5 good plunges; every plunge is a gross sound + noise.
  - Reset the clocks: 3 classroom clocks show a wrong time; drag the minute hand to the shift time. At low sanity every clock reads 3:33.
- **Shift time:** wing 1 starts 12:00 AM, +45 min per wing, 1 in-game minute per 10 s (`Config.Shift`). Clients draw clock hands (`ChoreFx`).
- **Assets added** (scripts removed, scaled, pivot floor, front -Z): WallClock (Ethan's clock), Vacuum, Broom, Plunger (Ethan's), StepLadder, DeskPhone, CrtMonitor, BatteryBox, BulbBox, SecurityCamera. Originals in ServerStorage.AssetBackups (`*_Original`).
- **Backdoor alert:** the desk phones, CRT monitors, a filing cabinet and even the "No scripts" CCTV camera from the Creator Store all had hidden scripts (`Runtime`/`ScreenBuilder`, fake `LightConfig` loaders). All removed before use; rejected the cabinet (plain box) and a broken monitor.
- **Bug fixed:** `Minigames.holding()` called `UserInputService:GetTouches()` (doesn't exist), erroring every frame in every mini-game when the mouse wasn't held.
- **Testing tip:** prompts only show when they're on screen; when testing with keyboard input, point the camera at the prompt first. Use the Dev remote `god` + `menace_freeze` so the Closer doesn't restart the wing mid-test.
- Commit `d5f112e`.

## 2026-10-19 (night session: feel, doors, assets, bathrooms)
- **Rule:** never use low-poly assets (added to [[_Index]] rules and [[Systems/Assets]]).
- **Doors:** DoorService. E opens/closes classroom doors; unlocked doors randomly slam shut / creak open out of sight; the Closer shoves shut doors open. Config.Doors.
- **Lock-the-door chore removed** (ChoreService kinds list). Also fixed: the trash chore silently never spawned since the imported TrashCan (parts are in a sub-model).
- **Feel:** PhantomSteps (footsteps only you hear, more at low sanity), ViewSway (camera + flashlight sway, stronger sprinting), Exertion (gasp, panting with Ethan's breathing-sound 124723360946174, edge vignette, blur, small heave), CloserGaze (looking at the Closer: red/desaturated, closing dark edges, screen tears, tremor, FOV pull, drone, first-sight sting). FirstPersonBody exists but legs are OFF (ShowLegs=false, Ethan's call).
- **Closer faster:** chase 16 -> 18 (+1.5 in blackout), walk 8.5 -> 10.5; always under the 20 sprint.
- **New assets** (Assets folder; originals in ServerStorage.AssetBackups): WaterHeater, JanitorCart, Radio (has invisible `Speaker` part), SchoolLockers (bank of 6, painted per bank from LOCKER_COLORS, doors welded to an invisible `LockerDoor` hinge part with `Hinge`/`Width` attributes; use Props.LockerDoorAt to swing any locker door), VendingMachine + VendingMachine_3 (soda machine removed at Ethan's request), ExitSign (hangs from ceiling, red glow), ToiletStall, Urinal, BathroomSink.
- **Bathrooms:** every wing has a BOYS and a GIRLS room (WingBuilder slots 2 and 3). Stalls on the far wall (most doors hang open), sinks + mirrors, urinals (boys) or a 2nd stall row (girls), blue/pink wall tile.
- **Watch out:** a Rojo sync reverted Config, client Main, Flashlight and WingService mid-session (they matched the files on disk). All of tonight's code changes live only in Studio until copied into `Documents/8 Wings Til Dawn/src`. Do that before the next sync.
- **Custodian room redone:** blocky Shelf x2, Ladder and the custodian desk removed (one fewer desk hide spot). New assets: SupplyShelf x2 on the back wall (shelf + spray cans, bleach bottles, duct tape, screwdriver; the wing key now rests on the first shelf's middle level via its `KeyOffset` attribute), BucketStack (buckets + paint roller, replaces the blocky one everywhere), MopBucket, LawnMower, GasCan, WaterJug, HandTruck, LeafBlower, CleaningCaddy. Placed with Props.Clutter(name, parent, cf).
- Still blocky in the custodian room (functional, need real models): MopSink (needs part `Faucet`), TrashChute (needs `Hatch`), BreakerPanel (needs `Door`, `Lamp`), SanityBoard, NoteBoard.
- **Backdoor again:** the imported MopBucket had the same fake-"Roblox Studio"-popup `Runtime` script (hidden in a part named `Hair`). Removed. Studio reloaded mid-session and it came back once (the place had been saved with it): always re-scan after a reload.
- **PA toned down:** PAService now has Say (important: new wing, arrival, key, taken, cleared, dawn), SayOnce (blackout warning: first blackout per wing only) and Ambient (staff count, "no running": only after 180 s of PA silence, max 2 per wing; Config.PA). The staff count used to fire every 45-75 s (its 200 s guard was always true). PAWhisper scare no longer uses the player's name.
- **Jumpscare is immediate:** CloserFx no longer fades the sound, blacks out, waits a random 'false calm' or turns you round to it. Sounds cut, lights die and it lunges in your face on the frame you're caught (measured 0.02 s, was 1.5-3.5 s). Strobe, black screen and 'you were taken' after it unchanged.
- **FireExtinguisher asset:** Ethan's glass-front cabinet replaces the blocky one (no scripts in it). Pivot = wall surface at the cabinet's middle height, back sunk 0.6 into the wall (semi-recessed), sticks out 1 stud. Original in AssetBackups.
- **WaterFountain asset:** Ethan's fountain replaces the blocky one (no scripts). Pivot = wall surface at floor, back flush to the wall, 2.4 wide x 4.3 tall, sticks out 2.6. Appears in the hallway 'lockers + fountain' dressing. Original in AssetBackups.
- **Task ideas** for the chore revamp were brainstormed (co-op fuse box, bulb replacement, alarm clocks, graffiti, plunger minigame, etc.); none built yet.

## 2026-10-19 (handoff)
- Wrote [[Handoff]]: current state, workflow gotchas and open items for the next conversation. Refreshed at the end of the night session (new systems, Studio-only code to copy into Rojo, pending decisions).

## 2026-10-18 (heavy assets + flashlight model)
- Bookshelf_2 merged from 172 parts into 3 unions (one per material, colors kept, box collision). Original kept in ServerStorage.AssetBackups.Bookshelf_2_Original.
- Asset variants can have a `SpawnWeight` attribute (default 1). Desk_2 (112 textured mesh parts, can't be merged without losing its texture) set to 0.3 → ~5 per wing instead of ~28. Server wing parts 11.5k → 9.9k.
- Imported black flashlight is now `Assets.Flashlight` (lens faces -Z, part "Lens", 0.8 studs, textured). Used for the first-person flashlight and the one in every character's hand (new shared module FlashlightModel). Lens glows when on. Import original in ServerStorage.AssetBackups.Flashlight_Import; duplicate AuxScene copies removed from Workspace.
- Committed 0cb5333. Place changes (assets) need the place saved/published.

## 2026-10-18 (performance pass)
- Profiled a Studio playtest: ~34 FPS, render CPU ~28 ms, GPU ~9 ms. Deleting the whole wing, all GUIs, post effects, lights or decals each changed it by ≤2 ms; client scripts cost 0.3 ms/frame; server physics ~0. So the Studio number is mostly Studio overhead (server + client + editor in one process), not the game. Judge real FPS in the Roblox player (publish, Shift+F5 for stats / Ctrl+F6 microprofiler).
- Fixes for real clients at max settings: only the 5 nearest ceiling lights cast shadows; fixture lights past 130 studs switch off (panel still glows); small/thin prop parts don't cast shadows, tiny ones don't collide, nothing in the wing has CanTouch. Wing shadow-casting parts 19,400 → ~2,800.
- Heaviest assets: Desk_2 (112 parts, basket rods), Bookshelf_2 (172 parts). Simplifying them is the next big win.
- Low-mode flashlight dimmed (6/1.5/4 → 3/0.6/2). Committed bc6831a. See [[Systems/Lighting]], [[Systems/Performance]].

## 2026-10-18 (low graphics mode)
- Flat/bright playtests were low graphics quality (no light shadows below slider 6). Added an automatic low lighting mode (shadowless narrow pools, wider flashlight). See [[Systems/Lighting]].

## 2026-10-18 (lighting matched to reference)
- Pools: 120° cone, range 14.5, brightness 3.5 (distinct floor circles + wall arches). Flashlight: crisp 28° circle, brightness 4, hot center. Values in Config.Lighting. See [[Systems/Lighting]].

## 2026-10-18 (scene lighting in Studio)
- Scene lighting now comes from the place's own Lighting properties (set in Studio, saved with the place); the server no longer overwrites them. Lighting was set to the previous night look. SunRays and DepthOfField removed from Lighting. Config.Lighting now only holds the lights themselves. See [[Systems/Lighting]].

## 2026-10-18 (lighting tuning)
- All lighting values moved into `Config.Lighting`; live tuning via `ReplicatedStorage.LightingTuning` attributes during playtests, `PrintForConfig` prints changes to paste back. See [[Systems/Lighting]].

## 2026-10-18 (lighting)
- Ceiling lights now throw a circle of light on the floor (downward SpotLight per fixture) with a faint shadowless fill; dark gaps between fixtures. Banners no longer cast shadows.
- Joining: screen stays black ~2 s while Roblox draws the lights, then fades in (lights were all on immediately; the delay was rendering).
- Flashlight beam is round (30° hotspot, soft 44° edge) instead of a wide 70° fan.
- Colorized `models/black-flashlight.glb` → `black-flashlight_roblox.glb` (not imported yet).
- Committed as a5a99b5. Details: [[Systems/Lighting]].

## 2026-10-18 (models + fixes)
- **Security:** the Toolbox canvas, easel and two paintings contained a hidden Studio-only backdoor script (fake "Roblox Studio" popup asking to paste a command). Removed; nothing else infected.
- Doors: 4.6-stud doorways, see-through vision lite (door asset rebuilt; slab cut around the glass). Closer pathfinding through doorways checked (17/18 ok; the failure was clutter, now kept out of doorways).
- Art room: paper art wall removed everywhere, easel+canvas asset, paint splats no longer overlap.
- New assets: TrashCan (corners only), 6 Backpack variants, 5 Flask variants, Easel. Radios/record players stand against walls at checked-empty spots.
- Two backpacks (Backpack_2, Backpack_5) imitate luxury brand patterns: possible moderation risk; consider dropping before publishing.
- Unused in Workspace: Starry Night and castle paintings.

## 2026-10-17/18 (assets)
- School is **Dawnridge High, Home of the Smiles** (welcome banner on the main hall start wall). Hallway banners still say GO TIGERS (to update).
- Real models now used for: classroom door + frame, 3 desk variants (chairs flip onto desks at night), bookshelves, book stacks/rows, pencil cups, board eraser, globe, wet floor sign, 5 posters.
- Locker bug fixed: an invalid font (`Enum.Font.Stencil`) crashed the Hiding script so the camera never locked and exit keys were ignored; it also stopped Ambience, CloserFx, Music and DevMenu from starting. Client controllers now start independently (one crash can't stop the rest).
- Wall z-fighting fixed (side-hall walls ran into the back hall's end walls; door frame and windows had coplanar faces).

## 2026-10-16 (bugfix + sound)
- Fixed: bulletin papers flickering (overlapping sheets were coplanar), held flashlight backwards, beam came from the camera (now from the flashlight: view-model light for you, hand light for others), doors clipping desks, wellness monitor text overflowing.
- Sound: added Sfx system + 19 generated placeholder sounds (hum, heartbeat, breathing, locker slam, whisper, PA chime, blackout power down/up, breaker, mop, trash, door lock, key, battery, exit, stinger). Need uploading; IDs go in Config.Sounds.

## 2026-10-16 (tools)
- Switched from Blender to **Claude Design** for all visual design work. Design is 2D, so meshes still come from the Creator Store, using Design sheets as the reference.

## 2026-10-16 (round 2)
- Playtest feedback: rooms/wings too small to split up, bulletin boards glitching, custodian room too empty.
- Added: branch corridor + bigger classrooms, first person lock, sprint/stamina, battery flashlight (replicated beam + held model), blackouts + breaker, sanity monitor, fuller custodian room.
- Fixed: bulletin boards and whiteboards were flush with the wall surface (z-fighting).

## 2026-10-16 (chore rework)
- Built the chore version: hallway with enterable classrooms + custodian closet, 3 chore types (mop, trash → chute, lock door), clipboard HUD, key → exit → next wing.
- Scares: 8 private scares (locker slam, open locker with your name, faceless student, your own avatar, local flicker, red light, PA whisper, fake spill on your clipboard).
- Visuals: detailed built-in props + **asset slots** so real models replace them ([[Systems/Assets]]). Ethan wants a professional, never-blocky look → real meshes needed.
- Next: playtest, then source/build real assets and the Closer.

## 2026-10-16 (pivot)
- Playtest: anomaly-spotting as the main objective wasn't fun.
- New main objective: **janitor chore list** per wing → key → next wing. See [[Design/Objective Brainstorm]].
- Anomalies became **private scares** ([[Design/Scares]]). Door voting dropped.
- Reusable from the current build: private-delivery system, sanity, flashlight, lighting, HUD. To replace: loop/vote service and the single-hallway map.

## 2026-10-16 (build)
- Rojo project + Git set up in `Documents/8 Wings Til Dawn`.
- Built the first playable loop: greybox hallway (layout randomized once per wing), loop manager, majority vote, hidden sanity + hallucinations, 6 anomalies, HUD with fade and light flicker.
- Closer is a placeholder (prints who it would take on the 3rd wrong call).
- Next: playtest, then the Closer model and kill.

## 2026-10-16 (later)
- Settled all pre-development decisions → [[Design/Decisions]].
- Renamed the game to **8 Wings Til Dawn**.
- Resolved: multiplayer uses the host's save slot; coins are earned only, plus a Robux earning boost.
- Next: Rojo project structure, then the hallway section and loop manager.

## 2026-10-16
- Chose co-op anomaly horror over a ROUNDS-like game (needs full lobbies, hard netcode) and a gambling game (Roblox rules ban playable simulated gambling).
- Setting: a school. Core idea: private anomalies and hallucinations create doubt between friends.

## 2026-10-21
- Brainstormed what taken players do. Ethan chose: CCTV spectate + an occasional option to scare friends. Wrote [[Design/After Death]] (design only, not implemented, open questions listed).
- Answered the After Death questions: punishment with cosmetic scares only (door slam, fake Closer jumpscare), no free rescue (revives purchase-only), room cameras for spectators but the CCTV chore stays halls-only, no Closer scaling. Updated [[Design/After Death]]. Still not implemented.
- Added rule: players always respawn with a flashlight ([[Design/After Death]] decision 8). Not implemented.
- Map rework: generated grid hallways, baked school surfaces (halls, bathrooms, ceilings), library wing off. Commit `a58c817`. [[Systems/Map]]
- Classrooms got the classroom-floor-wall.glb look (speckled VCT floor, cream block walls, tan base, popcorn ceiling). [[Systems/Map]]
- Dimmer hallway lights (half of room lights), thicker dark fog and smaller dark-adjust glow so you see less far in the dark. [[Systems/Lighting]]
- Fake chores removed (FakeSpill out of the scare pool). Commit `8d9b9ef`.

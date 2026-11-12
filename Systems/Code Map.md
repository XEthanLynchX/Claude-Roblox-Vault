# Code Map

Every script in the game, one line each: where to look before you search. Built 2026-11-12 from the scripts' header comments (133 scripts). Paths are repo paths; `.server` = Script, `.client` = LocalScript, plain = ModuleScript. Update this note when a script is added or removed. Line counts are rough, as a size guide.

**Entry points:** `src/server/Main.server.luau` (game server) · `src/client/Main.client.luau` (game client) · `src/lobby/server/Lobby.server.luau` + `src/lobby/client/LobbyClient.client.luau` (lobby). `Shared/Mode.luau` decides lobby vs game (public servers = lobby, reserved = game).

## Server: `src/server/Services/`
### Run of a shift
- **WingService** (650): runs a wing: build, chores, key, exit → next wing; spawns, taken players, wipe/run over.
- **WingBuilder** (2250): generates a wing: grid hallways, rooms, custodian room, office, cameras, vents, exit.
- **ShiftFlow** (330): shift board before a shift, intro wait, late joiners, run-over board.
- **CinematicService** (230): picks camera shots for the first-time intro.
- **DawnService** (90): dawn ending after the last wing.
- **CrewService** (110): counts clears for the lobby board; sends the crew back to the lobby.
- **HostSave** (130): hosted private shifts: save slot and wing reached.
- **AfkService** (130): AFK kick to the lobby; the chore list shrinks.
- **TutorialService** (170): who has done the first-time tutorial (DataStore).
### Chores
- **ChoreService** (2070): the chore list and every chore's prompts; shared chores, `touch()` ("on it"), `shrinkFor`, Closer undo/spills.
- **MinigameService** (140): server side of task mini-games (anti-cheat: distance, min time).
- **PhoneQuestions** (100): office phone quiz questions.
- **InventoryService** (560): 3-slot hotbar, give/take/drop/spawn items, OnLost handlers.
- **Carry** (65): welds carried props (trash bag, key) to the hand.
### The Closer and fear
- **CloserService** (3520): the Closer's AI: menace phases, arrival, rounds, hunts, catching, vents, jumpscare.
- **CloserLook** (210): the Closer's look per wing.
- **MenaceService** (140): the menace meter that drives the Closer.
- **NoiseService** (55): noises the menace meter and the Closer hear.
- **AwarenessService** (40): when the Closer last saw each player.
- **HideService** (480): desk and locker hiding, "truly hidden" rules, skill check trigger.
- **DoorService** (490): classroom doors, self-slams, the Closer shoving doors.
- **ScareDirector** (260): schedules private scares per player by sanity.
- **SanityService** (60) · **SanityBoard** (60): hidden sanity; the custodian-room monitor.
- **BlackoutService** (180): random blackouts and the breaker.
- **FlashlightService** (150): server-side flashlight beam others see.
- **RoomSounds** (110): heavy dragging sounds in empty rooms.
- **MusicPlayerService** (280): radios/record players that switch on by themselves.
- **PAService** (140): PA announcements with subtitles.
- **SchoolerService** (1090): the Schooler (**being removed**, Redesign Phase 1).
- **LibraryBuilder** (780) · **LibraryService** (750): Blackthorn Library level (switched off; was the Schooler's).
### Money, progress, players
- **PlayerData** (400): saved profile (coins, items, receipts), session lock.
- **ShopService** (470): the supply shop (lobby, and in game while taken).
- **Receipts** (50): the single ProcessReceipt dispatcher.
- **AfterDeathService** (250): after-death shop: blackout, scare a friend, give battery, R$25 revive.
- **OfferService** (100): pop-up offers (battery, sanity candy, restock).
- **GadgetService** (620) · **GadgetState** (90): gadgets (tripwire, decoy, stopwatch...) and what they're doing for the Closer's senses.
- **TitleService** (200): name tags and the stats behind them.
- **DevService** (290): dev menu commands (Ethan's account only).

## Client: `src/client/Controllers/`
### HUD, menus, screens
- **HudController** (710): HUD, chore list on the clipboard ("Sam is on it"), messages, bars.
- **JournalBook** (790): the 3D journal (Tab/J): chores, job, notes, staff.
- **HotbarController** (450): hotbar and the item drawn in first person.
- **PromptUi** (510): restyled "press E" prompts; LookAt prompts.
- **ShiftScreens** (1390): shift board, wing badge, run-over board.
- **ShopController** (1480): shop window (lobby + taken).
- **OfferPopups** (380): pop-up offer cards.
- **SpectateController** (740): after you're taken: CCTV cameras + after-death shop.
- **TutorialController** (730): first-time tutorial coach marks.
- **CinematicController** (710): intro film.
- **DawnController** (880) · **DawnSet** (280): dawn ending film and its outside set.
- **DevMenu** (240): F4 dev menu.
- **UiTheme** (760): the shared look for every screen.
- **TitleStyle** (170) · **TitleTags** (190): how name tags look; tags over heads.
- **WorldPreload** (130): loads the school in the background on the shift board.
### Player feel
- **Movement** (110): walk/sprint/crouch, camera height, FOV.
- **FirstPersonBody** (90): shows your legs in first person.
- **ViewSway** (90): head sway while walking.
- **Exertion** (160): out of breath when exhausted.
- **Flashlight** (560): flashlight, battery, beam.
- **Hiding** (590): desk and locker views.
- **SkillCheck** (360): the locker hold-your-breath skill check.
- **Minigames** (2110): every task mini-game screen.
- **MopController** (190): mopping hint and progress.
- **ToolAnim** (180): tool-use animations (yours and others').
- **ChoreFx** (160): chore visuals (clocks, 3:33 at low sanity).
### The Closer and fear (client side)
- **CloserSkin** (950): the skinned Night Custodian, animated from CloserAnims.
- **CloserFx** (1060): Closer details, the death jumpscare.
- **CloserPresence** (340): the black "presence" look.
- **CloserGaze** (240): what looking at the Closer does to you.
- **CloserDarkness** (70): lights near the Closer go dark.
- **HuntPA** (80): PA countdown to a hunt.
- **ScareController** (70): plays private scares.
- **PhantomSteps** (230): fake footsteps only you hear.
- **SanityEffects** (35): colour drains as sanity drops.
- **Ambience** (110) · **MusicController** (270): hum, heartbeat, the score layers.
- **WindowRain** (80): rain on classroom windows.
- **SchoolerFx** (290): Schooler visuals (**being removed**).
- **LibraryFx** (360): Library level visuals (off).

## Shared: `src/shared/`
- **Config** (940): every tuning number. Change numbers here, not in systems.
- **Remotes** (60): owns every RemoteEvent.
- **Props** (1640): every prop; uses `ReplicatedStorage.Assets` models when present.
- **Items** (240): hotbar items: model, hold, view, floor offsets.
- **Gadgets** (290): gadget shop rows and models.
- **LightFx** (460): client owner of the ceiling lights (power, flicker, red, off).
- **Surfaces** (85) · **Materials** (25): baked school surface materials; safe material lookup.
- **Sfx** (70) · **Voice** (100) · **GlitchVoice** (260): sounds by name; voice lines; the breaking-up PA effect.
- Voice line tables, all generated by `tools/make_voices.py` or `make_pa.py`: **PALines**, **PAExtraLines**, **CreepyLines**, **HuntLines**, **TutorialLines**.
- **CloserAnims** (generated, large): the Closer's animation clips from Blender.
- **ScareUtil** (610): helpers for client scares.
- **Scares/**: CrawlingCloser, FakeCloser (after-death purchase), FakeSpill (unused), FloatingCloser, LocalFlicker, LockerSlam, LyingCloser, OwnAvatar, PAWhisper, RedLight, SeizingCloser, StallFeet, StillStudent, TheCount, WindowPress.
- **ChoreIcons**, **FlashlightModel**, **Lore** (lobby wall pages), **Titles** (name tag list), **Mode** (lobby vs game).

## Lobby: `src/lobby/`
- **server/Lobby.server** (320): break room server: shift doors, teleporting crews.
- **server/BreakRoom** (230): builds the break room.
- **server/ShiftDoors** (620): the 5 NIGHT SHIFT doors and their waiting rooms.
- **server/HostService** (290): HOST GAME private shifts.
- **server/Boards** (290): MOST CLEARS and DONOR boards.
- **server/GearShelf** (140): gadget shelves.
- **server/LoreWall** (80): lore pages on the wall.
- **client/LobbyClient.client** (870): lobby HUD, invites, countdown, lore reader, donations.
- **client/HostMenu** (630): HOST GAME menu.

## Archived
- `archive/shared/Scares/OpenLocker.luau`: removed from the game; kept for reference.

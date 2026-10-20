# Handoff

Read this first when picking the project up in a new conversation. Updated 2026-10-19 (afternoon: hotbar + new chores).
Then read [[_Index]] (rules), [[Tasks]] and the newest entries in [[Log]].

## Where things are
- **Project folder:** `Documents/8 Wings Til Dawn` (Rojo + VS Code + Git). Rojo syncs only `src`:
  - `src/shared` → ReplicatedStorage.Shared
  - `src/server` → ServerScriptService.Server
  - `src/client` → StarterPlayerScripts.Client
- **Collaboration:** Ethan works with a friend through Team Create. Only one person can run Rojo sync at a time; see [[Systems/Conventions]].
- **The map is built at runtime** by `WingBuilder` (server), using `Props.luau`. Nothing in the Workspace is the map.
- **The place file holds things that are not in Git.** Save or publish the place to keep changes to them.
  - `ReplicatedStorage.Assets`: the imported 3D models, used as asset slots. See [[Systems/Assets]].
  - `ServerStorage.AssetBackups`: originals of assets Claude edited (Bookshelf_2_Original, Flashlight_Import).
  - `ServerStorage.Closer`: the monster model.
  - Lighting properties: the scene look is set in Studio, not by scripts.
- **Vault:** this folder. Design notes are under `Design/`, technical notes under `Systems/`.

## How Claude works on it
- **Studio access:** Claude reaches Studio through the Roblox Studio MCP tools.
  - `execute_luau` runs in the Edit, Server or Client datamodel.
  - `screen_capture` only works in Edit mode.
  - `start_stop_play` starts and stops playtests.
- **Edit-mode module caching:** `require` can return stale modules after a Rojo sync. Build previews from fresh clones in `ReplicatedStorage.__Tmp`.
- **Clean up previews:** always remove `__Preview`, `__Beam` and `__Tmp`, and restore Lighting and the camera, before a playtest. They get copied into the playtest otherwise.
- **Checking code:** there is no luau-compile. A playtest with no errors in the console is the check. The ignorable noise is:
  - UIBlox FoundationProvider warnings;
  - asset 203785492 "type does not match".
- **Git from the device shell:**
  - Deleting isn't allowed there, so git leaves `.lock` files behind. Move them aside: `mv .git/HEAD.lock .git/stale-HEAD-<time>`.
  - End commit messages with the Co-Authored-By and Claude-Session lines.
- **Studio FPS is not real FPS.** In a Studio playtest, server + client + editor share one process, so it sits at about 34 FPS even with the map deleted. Real numbers come from the Roblox player: Shift+F5 for frame stats, Ctrl+F6 for the microprofiler.
- **Ethan's standing rule:** anything relevant goes into this vault, every time.

## Current state (as of 2026-10-19)
- **Playable loop exists:**
  - chores and the wing key;
  - the Closer AI (menace phases, hunting, lockers to hide in);
  - private scares and sanity;
  - blackouts and the breaker;
  - flashlight with batteries;
  - music players and the journal.
- **Lighting** (see [[Systems/Lighting]]):
  - ceiling fixtures throw floor pools from a SpotLight called `Pool`;
  - all values live in `Config.Lighting` and can be tuned live during a playtest through `ReplicatedStorage.LightingTuning`;
  - a low graphics mode switches on automatically below quality 6 (no shadows there).
- **Performance** (see [[Systems/Performance]]):
  - only the 5 nearest lights cast shadows;
  - lights past 130 studs switch off;
  - small props don't cast shadows;
  - Bookshelf_2 is merged into 3 parts;
  - Desk_2 spawns at weight 0.3.
- **Flashlight:** both the first-person flashlight and the one in characters' hands use the imported model (`Assets.Flashlight`, built by `src/shared/FlashlightModel.luau`).
- **School identity:** "Dawnridge High, Home of the Smiles".
- **Last commits:**
  - `0cb5333`: flashlight model and weighted variants;
  - `eaea53d`: Ethan's "changes";
  - `bc6831a`: performance pass.

- **Added in the 2026-10-19 night session** (details: [[Log]] newest entry):
  - DoorService: E opens/closes classroom doors; haunted doors slam/creak; the Closer shoves doors open.
  - Client feel modules: PhantomSteps, ViewSway, Exertion (breathing asset 124723360946174), CloserGaze. FirstPersonBody exists but legs are OFF.
  - The Closer is faster (chase 16-18, walk 8.5-10.5, always under the 20 sprint) and the jumpscare is immediate (0.02 s).
  - Boys and Girls bathrooms in every wing; the custodian room was rebuilt with real supply models (the wing key rests on the first SupplyShelf).
  - PA is infrequent (Config.PA) and no line uses the player's name.
  - Lock-the-door chore removed.
  - Imported assets now in use: WaterHeater, JanitorCart, Radio, SchoolLockers (per-bank colors, working hide animation), VendingMachine + VendingMachine_3 (soda machine removed), ExitSign, ToiletStall, Urinal, BathroomSink, SupplyShelf, BucketStack, MopBucket and other clutter, FireExtinguisher, WaterFountain.

- **Added 2026-10-19 afternoon** (details: [[Log]] newest entry, [[Systems/Inventory]]):
  - 3-slot hotbar: 1/2/3 or wheel, G drops, flashlight is an item that only shines in hand; trash bags and the wing key are items.
  - Office room in every wing: PA phone, security monitors, flashlight charger, master clock.
  - 10 new chores: dead lights, co-op ladder, vacuum/broom, charger restock, cameras, graffiti, PA phone quiz, chairs up, plunger, clocks.
  - Ethan's clock/plunger/broom/vacuum are Assets; all code is in Git (commit `d5f112e`); Studio and src matched exactly at the end of the session.
  - Git delete permission was granted for the project folder this session, so commits no longer leave `.lock` files (re-request it in a new session).

- **2026-10-19 evening:** horror UI (task screens, breath line, battery gauge, hotbar) committed as `52ebad5`. Studio and `src` matched exactly afterwards, so a Rojo sync is safe. See [[Systems/UI]].

## Open items / next steps
- **Night Custodian model** (evening 2026-10-19): modelled, rigged and animated in Blender (`models/NightCustodian.blend` / `.fbx`). Late evening: mop removed, realistic textures baked, Roblox-ready FBX + 1024 PBR maps in `models/textures`. **Done 2026-10-20:** it is now the Closer in Studio (commit `61026a0`); see [[Design/The Closer]]. Ethan must save the place (the model lives in ServerStorage, not in src). Not yet tested: multiplayer (two clients seeing different crawl states), hearing the new sounds in a real chase. See [[Systems/Night Custodian Model]].
- **Save/publish the place** so the new assets (WallClock, Vacuum, Broom, Plunger, StepLadder, DeskPhone, CrtMonitor, BatteryBox, BulbBox, SecurityCamera) and the backdoor removals persist. Re-scan imports for scripts (`Runtime`, `ScreenBuilder`) after any reload.
- **New chores need a real playtest, ideally with 2 players** (the ladder only appears with 2+; it couldn't be tested solo in Studio). Tune numbers in `Config.ChoreTuning` / `Config.Menace`.
- **Held items' looks are unchecked:** Studio screenshots don't work during a playtest, so `Items.Defs` Hold (third person) and View (first person) offsets are first guesses. Ethan should look and say what's off. The vacuum Hold was raised once (it sank into the floor).
- **The office layout hasn't been looked at in a screenshot** (Studio's viewport wasn't rendering at the end of the session). Placement was checked with numbers (phone/monitors on the desk tops, ladder on the floor, clock hands turning the right way).
- **Decisions waiting on Ethan:** whether the stylized CleaningCaddy breaks the no-low-poly rule; whether to remove the username text from the 'open locker' scare; models for the remaining blocky custodian props (MopSink, TrashChute, BreakerPanel, SanityBoard, NoteBoard).
- **Not fully verified:** locker-hide inner view (door see-through while hidden), sprint sway amount, exit sign in-game, new-wing timing.
- **Unverified changes:**
  - Real-player FPS at max settings hasn't been checked since the performance pass. Publish, join, check Shift+F5.
  - The low-mode flashlight was dimmed (brightness 3, edge 0.6, throw 2) but Ethan hasn't confirmed it looks right.
  - Studio's saved graphics quality reported manual level 1 in playtests. Ask Ethan if that's intended.
- **Desk_2:** to restore its normal spawn rate, the real fix is in Blender (merge the basket rods into one mesh, then re-import). Unions would drop its texture.
- **Offered, not yet requested:**
  - switch the hallway banners ("GO TIGERS") and PA lines to Dawnridge branding;
  - drop the brand-like backpacks (Backpack_2, Backpack_5) before publishing;
  - move the Assets into Git as `.rbxm`;
  - add `.gitattributes` and StyLua;
  - find the bad asset id 203785492.
- **Leftovers:** unused items still in the Workspace are AuxScene (The_Closer, Journal), a Journal and 3 Painting models. These paintings were cleaned of a Toolbox backdoor script earlier.
- **Bigger roadmap:** see [[Tasks]]. Main items:
  - the two-place setup (lobby + game);
  - tuning the menace numbers;
  - real sounds;
  - revive and spectate;
  - the first playable target is the end of October.

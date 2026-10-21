# Handoff

Read this first when picking the project up in a new conversation. Updated 2026-10-20 evening (after the first-time tutorial).
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
  - asset 203785492 "type does not match";
  - asset 6160151677 "not authorized" (the unused backup Classic Radio in AssetBackups).
- **Git from the device shell:**
  - Deleting isn't allowed there, so git leaves `.lock` files behind. Move them aside: `mv .git/HEAD.lock .git/stale-HEAD-<time>`.
  - End commit messages with the Co-Authored-By and Claude-Session lines.
- **Studio FPS is not real FPS.** In a Studio playtest, server + client + editor share one process, so it sits at about 34 FPS even with the map deleted. Real numbers come from the Roblox player: Shift+F5 for frame stats, Ctrl+F6 for the microprofiler.
- **Ethan's standing rules:** anything relevant goes into this vault, every time. Never use low-poly assets. Prefer the Roblox Studio MCP over computer use; Ethan does model imports himself.
- **Studio MCP notes (2026-10-20):**
  - The studio_id changes when Studio restarts: call `list_roblox_studios` first.
  - Script edits only work in Edit mode, so stop the playtest first. For big edits, string-replace `.Source` with `execute_luau` (plain `string.find`); `multi_edit` chokes on long payloads.
  - `screen_capture` does work during a playtest now (Client view).
  - `user_keyboard_input` didn't reach the game on 2026-10-20 (no E/C). Requiring a service from `execute_luau` creates duplicate RemoteEvents via Remotes.get; fire `ReplicatedStorage.Remotes.<name>` directly when testing.
  - `user_mouse_input` actions are `moveTo` / `mouseButtonClick` (mouse_button "left"); `instance_path` like `LocalPlayer.PlayerGui.Tutorial.Holder.Card.Skip` is reliable, raw x/y is not. Give buttons unique names so paths resolve.
  - Requiring a controller from `execute_luau` gives a separate module instance (not the running one).
  - The Studio DataStore fails unless Game Settings > Security > Studio API access is on.
- **Exporting Studio code to `src`:** return `HASH <path> <hash> <len>` lines plus `@@FILE path@@BEGIN@@source@@END@@` blocks from `execute_luau` (keep it under ~100k chars per call), parse the saved tool-result in the container, check the hash (`x=(x*31+byte)%2147483647`), then `device_commit_files` with force. Commit in ONE git command with explicit paths (`git commit -F - -- <paths>`); a separate `git add` leaves an `index.lock`. Move stale `.lock` files aside before and after.
- **Other people edit Studio at the same time** (Ethan, his friend, other Claude sessions). Make targeted edits only; never overwrite a whole script you didn't just read.

## Current state (as of 2026-10-21)
- **Map rework (2026-10-21, `a58c817`):** every wing is now a generated grid of hallways with the baked school surfaces; library wing off. See [[Systems/Map]]. MaterialService `School*` variants live only in the place file: **save the place**.
- **Previous commit:** `149fc4a` hiding fixes, 0.8-scale players, voice ids, CCTV Closer. Before it: `423ecf5` every spoken line voiced, `26e8a1c` + `815ed0e` school backpacks / coat hangers, `226f2d5` Studio-only Schooler/Library scripts exported to src, `7bce176` first-time tutorial, `62faa73` Closer arrives later, `32dd8b5` cleanup pass (gym chains, flags, thermostat, vending blackout, dev spawns), `4f4129f` gym exit doors, `a9c3a97` bug pass + glossy look, `75fc4b4` 17-point chore pass, `61026a0` Night Custodian is the Closer.
- **Playable loop:** chores and the wing key, the Closer (Night Custodian model, menace phases, hunting, lockers), the Schooler (second monster, never in Wing 1), private scares and sanity, blackouts and the breaker, flashlight with batteries, 3-slot hotbar, 3D journal, first-time tutorial.
- **Tutorial (new, 2026-10-20):** `TutorialService` (server) + `TutorialController` (client). Plays the first time a player is in Wing 1; finishing or skipping saves `TutorialDone` in DataStore `PlayerFlags_v1`; [H] replays it. 11 kid-friendly cards with HUD rings and world highlights. While it runs (`ReplicatedStorage.TutorialActive`) menace, Closer arrival, blackouts and scares pause and the flashlight doesn't drain. See [[Systems/UI]] and the newest [[Log]] entry.
- **Journal:** held larger (fills most of the screen), hotbar + held item hide while open (`JournalOpen` attribute), bottom-right key hints.
- **Closer timing:** first arrival needs menace >= 40 AND `Config.Closer.ArriveAfterSeconds` (150 s Wing 1 → 75 s Wing 8).
- **Hiding (2026-10-20 late):** `HidePose` attribute: Locker = invisible to others, Desk = everyone sees a knee-hug pose. Players are 0.8 scale (Config.Characters.Scale). Avatars use AnimationConstraints (not Motor6Ds).
- **CCTV:** feed figures are the real Night Custodian; 35% leap into the lens.
- **Backpacks + voice (2026-10-20 evening):** school backpacks against walls / under desks / on coat hangers ([[Systems/Assets]]); every spoken line voiced, normal in the PA voice, creepy in a glitchy one ([[Systems/Voice]]).
- **Look:** glossy materials via MaterialService overrides (CeramicTiles → GlossyVinylTile1, Plaster → GlossyPaintedPlaster) and `Lighting.EnvironmentSpecularScale` 1 / `EnvironmentDiffuseScale` 0.25. These live in the place file, not Git.
- **Blackouts** also kill "powered props" (vending machines etc.) via `LightFx.powerProps`; the exit sign stays lit.
- **Dev menu:** god, menace, closer pause, wing restart, give items, blackout, and "Spawn a monster" (Closer / Schooler).

## Studio-only code: now in Git (2026-10-20 evening)
- `SchoolerService`, `LibraryBuilder`, `LibraryService` (server), `SchoolerFx`, `LibraryFx` (client) and Studio's version of `DevService` (with `schooler_spawn`) were exported to `src` and committed as `226f2d5`. Every file's hash matched the Studio source.
- A Rojo sync is now safe for these. Still true: any script edited only in Studio is overwritten by the next sync, so export after every change (see [[Systems/Conventions]]).
- Not in Git, still only in the place file: `ReplicatedStorage.Assets`, `ServerStorage.AssetBackups`, `ServerStorage.Closer`, `ServerStorage.LibraryKit` and the Schooler model, Lighting properties, MaterialService overrides.
- `models/school-backpack.glb` is untracked in the project folder (Ethan's file, left alone).

## What Ethan needs to do
- **Save/publish the place** (tutorial scripts are in Git, but materials, assets and the Studio-only scripts above are only in the place).
- Turn on **Game Settings > Security > Enable Studio Access to API Services**, or the tutorial shows every Studio session.

## Open items / next steps
- **Tutorial:** not yet tried with 2 players (one in the tutorial pauses the school for everyone; 10-minute safety stop). Mobile/gamepad: cards say "Press F / Tab"; touch players have no keyboard hints yet. ExitDoors highlight wasn't seen on screen in the test (it's far away; AlwaysOnTop should still show it).
- **Playtests needed:** new chores with 2 players (ladder only spawns with 2+), menace/Closer tuning, the Schooler in Wings 2+, held-item looks in third person (vacuum still odd).
- **Night Custodian:** multiplayer crawl states and chase sounds untested. See [[Systems/Night Custodian Model]].
- **Decisions waiting on Ethan:** whether the stylized CleaningCaddy breaks the no-low-poly rule; remove username text from the 'open locker' scare?; models for blocky custodian props (MopSink, TrashChute, BreakerPanel, SanityBoard, NoteBoard).
- **Unverified:** real-player FPS at max settings (publish, Shift+F5); low-mode flashlight brightness; Studio's saved graphics quality level 1.
- **Desk_2:** real fix is in Blender (merge basket rods), then re-import.
- **Offered, not yet requested:** Dawnridge branding for banners/PA; drop brand-like backpacks (Backpack_2, Backpack_5); Assets into Git as `.rbxm`; `.gitattributes` + StyLua; find bad asset id 203785492.
- **Leftovers in Workspace:** AuxScene (The_Closer, Journal), a Journal and 3 Painting models; the old Schooler model backup renamed "School-head monster copy (was in Workspace at the hall start)".
- **Bigger roadmap:** see [[Tasks]] (two-place lobby + game, real sounds, revive + spectate; first playable target end of October).

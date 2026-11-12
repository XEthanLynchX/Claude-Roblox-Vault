# Workflow: how Claude works on the game

Read after [[_Index]] at the start of a session. Ethan's rule (2026-11-12): **Rojo first for code, the Studio MCP for everything else.**

## Session start (every time)
1. Read [[_Index]] → [[Handoff]] → the newest [[Log]] entry → whatever phase or note the task is about.
2. **Which computer?** Ethan works on a Mac and a Windows PC (see Machines). The connected folders tell you which.
3. **Folders connected?** The vault, plus the game repo (`8-Wings-Til-Dawn`). If the repo isn't connected, ask Ethan to add it ("+" → Add folder). Claude can't connect folders itself.
4. **Studio:** `list_roblox_studios` (the id changes every time Studio restarts), then `get_studio_state` (should be Edit).
5. **Rojo running?** Ask Ethan to run `rojo serve` in the repo folder and connect the Rojo plugin in Studio. Check by editing nothing and comparing one file's hash with Studio (see Checking Studio = src).
6. **Git:** `git status` and `git log -1` in the repo. Uncommitted changes from Ethan or his friend are theirs: don't overwrite or commit them without asking.

## Writing code: Rojo first
- **Edit the files in `src`** with device_bash (a short Python read-modify-write that asserts each replaced string occurs exactly once). Never retype a whole file from tool output.
- **Keep line endings:** files on the Windows PC are CRLF, on the Mac LF. Read, normalise to `\n`, edit, write back in the file's original style.
- Rojo pushes each save into Studio within a second. **Confirm it arrived** with the hash check below before testing.
- Rojo maps (`default.project.json`): `src/shared` → ReplicatedStorage.Shared, `src/server` → ServerScriptService.Server, `src/client` → StarterPlayerScripts.Client. The lobby uses `lobby.project.json` (`src/lobby/server`, `src/lobby/client`).
- Script file names: `X.luau` = ModuleScript, `X.server.luau` = Script, `X.client.luau` = LocalScript.
- **Not code, so not in Git:** `ReplicatedStorage.Assets`, ServerStorage models and backups, Lighting properties, MaterialService variants. Those are changed through the MCP and kept by saving the place.
- **Team Create:** only one person may run Rojo at a time. If Ethan's friend is syncing, coordinate first.

### Fallback: Rojo not running
Edit in Studio through the MCP (`execute_luau` string-replace on `.Source`, exact and unique matches), then **at the end of the phase** export: Ethan does File → **Download a Copy** into the repo folder, and Claude reads it with the binary reader (below), writes `src`, checks the digest against Studio, and commits. Never leave Studio-only edits un-exported: a later Rojo sync wipes them (it happened in Oct–Nov 2026: Git fell three weeks behind).

## The Studio MCP: everything that isn't writing code
- **Playtests:** `start_stop_play`, then `execute_luau` in the `Server` or `Client` datamodel, `get_console_output` for errors, `screen_capture` for a look. Stop the playtest before editing.
- **Inspecting:** `search_game_tree`, `inspect_instance`, `script_read` / `script_grep` (reads Studio, so it also verifies Rojo synced).
- **Place-only work:** models, assets, lighting, MaterialService.
- **Not computer use:** Ethan prefers the MCP over controlling his computer.

### Playtest recipes (solo)
- **Get through the shift board and intro:** in the Client datamodel, loop: if `ShiftPhase == "PreShift"` fire `Remotes.Shift:FireServer("ready", true)`; if `"Intro"` fire `("skipIntro")`; stop at `"Playing"` with a character.
- **Give an item:** `Remotes.Dev:FireServer("give", "Mop")` from the Client (dev commands: see DevService; e.g. `chores_complete`, `wing_skip`, `closer_hunt`, `blackout_start`, `batteries`).
- **ProximityPrompts can't be triggered from execute_luau** (`InputHoldBegin` does nothing). Test through remotes (e.g. `Remotes.Mop:FireServer("start")`) or by setting attributes on the server.
- **Fake a second player's state:** set attributes on the server (e.g. a chore's `ActiveBy`) and read the client's GUI. Real 2-player tests need Ethan + a friend.
- **Ignore this console noise:** "Requested asset is archived" (old sound ids), asset 203785492 "type does not match", asset 6160151677 "not authorized", UIBlox FoundationProvider warnings.

### Checking Studio = src
- Hash used everywhere: `x = (x*31 + byte) % 2147483647` over the file bytes / `.Source`. Compute it in Studio (`execute_luau`) and on disk (Python) and compare. For the whole codebase, hash the sorted `path\thash\tlen` lines on both sides (Studio side: walk the five Rojo roots; LocalScript → `.client`, Script → `.server`).
- **Reading a downloaded place (.rbxl):** binary format with zstd chunks. The device shell has `libzstd` (use it via `ctypes`; no downloads needed). Parse INST (class + referents), PROP (`Name`, `Source` as type 0x01 strings), PRNT (parents); referents are interleaved big-endian zigzag ints, cumulative. The reader written on 2026-11-10 wasn't kept; rewrite it from this description (~120 lines).

## Finishing a piece of work
1. Playtest: no new console errors, plus the phase's "Done when" checks.
2. Commit in the repo: `git add -A <paths>` then `git commit -F -` with the attribution lines. The repo folder needs **delete permission** for git's lock and temp files (`device_request_delete_permission` once per session). Without it, move stale `.lock` files aside.
3. **Pushing:** the device shell has no GitHub login. Ethan pushes (GitHub Desktop / VS Code / `git push`).
4. Vault: tick the phase in [[Design/Redesign Plan]], add a [[Log]] entry (newest first), update the system notes the phase names, and update [[Handoff]]'s "Now" section.
5. Tell Ethan what changed, what was tested, and what still needs a 2-player test or an upload from him.

## End of every chat (Ethan's rule, 2026-11-12)
Before the last reply of **every** chat, even one where no phase was finished:
1. **Sync check:** compare the whole-codebase digest of Studio with `src` (see Checking Studio = src). If Rojo is running, they should already match.
2. **Anything only in Studio** (an MCP edit, a quick fix, a script Ethan or his friend added in Studio): bring it into `src`. Either have Rojo running and copy the changed script's source into its file (when it's small, via an exact edit), or use the fallback (Download a Copy → the reader → `src`). Then re-run the digest until both sides match.
3. **Anything only in `src`** (Rojo wasn't connected when Claude edited the files): ask Ethan to connect Rojo so Studio picks it up, then re-check.
4. **Commit** everything that changed (with the attribution lines) and remind Ethan to push.
5. If any of this can't be done (no repo folder, Studio closed, Rojo off), **say so plainly in the last reply** and add it to [[Handoff]] → Waiting on Ethan, so the next chat starts by fixing it.
6. Update [[Handoff]] → Now and [[Log]].

## Machines
| | Mac (`ethans-macbook-air-local`) | Windows PC (`ethanspc`) |
|---|---|---|
| Vault | `/Users/ethanlynch/Documents/GitHub/Claude-Roblox-Vault` | `C:\Users\Ethan Lynch\Documents\Github\Claude-Roblox-Vault` |
| Game repo | `/Users/ethanlynch/Documents/GitHub/8-Wings-Til-Dawn` | probably `C:\Users\Ethan Lynch\Documents\Github\8-Wings-Til-Dawn` (check) |
| Line endings | LF | CRLF |
- The device shell is a Linux VM on that machine: it can't run Ethan's Mac/Windows programs (Rojo, Studio). External downloads (binaries) are blocked; system tools like `python3`, `zstd`, `git` are there.
- Inside device_bash, connected folders are at `$HOME/mnt/<folder name>`.
- Uploads (sounds, voice packs, models) are always Ethan's: Claude prepares the file and tells him what to upload, then puts the id in `Config.Sounds` / Assets.

## Studio MCP notes (from the old Handoff, still true)
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

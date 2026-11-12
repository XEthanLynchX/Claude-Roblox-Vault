# Log

Decisions and end-of-session recaps, newest first.

## 2026-11-12
- Ethan: everything that says 8 Wings Til Dawn (voice lines, art, text) changes to 5 Wings Til Dawn. Phase 1 step 6 is now a full rename sweep: code text, voice packs, repo art, place-file text and images, the Creator Dashboard, the vault.
- Vault reorganised: START HERE `_Index`, new [[Systems/Workflow]] (Rojo first, MCP for the rest, end-of-chat sync rule) and [[Systems/Code Map]]; Handoff and Tasks rewritten; October log, old tasks and old handoff moved to `Archive/`.
- Ethan: players get batteries at the start of every wing. Added to Phase 12: full flashlight + 1 spare, capped at 3 (`Config.Flashlight.StartSpares`), also on returns through a badge, the Lost & Found or a revive.

## 2026-11-10 (Ethan answered all 21 open questions)
- Name "5 Wings Til Dawn"; thumbnails at launch (Ethan picks). **Remove the Schooler** (broken), now in Phase 1. Monsters: only the five tied to the School Rules stay ([[Design/Monsters]] rewritten). Only Lost & Found badges revive instantly. Revive R$25 once per run. Spectate over the shoulder, with care for lockers, desks and walls. The CCTV chore moves into the Security Desk, which also gets door control, a PA intercom and a teammate map. Closer: loom + taller model with a door duck. Gloves: blue/black/red/yellow, one colour per player, made by Claude in Blender. Dead-flashlight shake makes noise. Tool area: labelled, matching the scenery; hangable tools on a wall rack, the vacuum on a marked floor spot. Roll Call: lockers/desks only. Rule order kept. The New Hire copies you in solo. **No wing themes** (Phase 10 is now atmosphere only). Keep all conditions/events. Interrogation: 6 s, an "I won't tell" button, the named friend is hunted via the vents and survives only hidden (skill check). All four Phase 16 close calls; Ethan's hold-the-door rules (the Closer sprints to a shut door, 5 s mash, harder each wing). New Phase 17: lobby showpiece (default idea: the Closer chained in the boiler room behind wired glass, reacting to players and door countdowns). Full test is now Phase 18.

## 2026-11-10 (Git caught up with Studio; redesign additions)
- Ethan saved a copy of the place (`Up-to-date-copy-11-10.rbxl`, gitignored) into the repo. Claude read it with a small binary-format reader (zstd chunks via the system libzstd; script at `~/work/rbxl.py` in the device shell, not kept) and wrote all 133 scripts to `src` (52 new, 48 changed). The digest of every path+hash+length matched the open Studio session exactly. `OpenLocker.luau` (gone from Studio) moved to `archive/` so a Rojo sync won't restore it. Commit `a02b0e2` (includes Phase 2). **Not pushed:** the device shell has no GitHub login; Ethan pushes.
- How to export next time: the same way (Ethan: File → Download a Copy into the repo folder), or `rojo syncback` on his Mac.
- Still only in the place file (not code): `ReplicatedStorage.Assets`, `ServerStorage` models/backups, Lighting, MaterialService. `ServerStorage.__Phase2Backup` holds the pre-Phase-2 sources of 4 scripts; delete it after the 2-player test.
- Redesign Plan additions (Ethan): Phase 14 tools + item fixes (vacuum sinks into the floor: `Items.Vacuum.Floor` is identity; trash bag is a black Ball part `Props.TrashBag`, hence the black circle in the hotbar; tools hard to find among the custodian-room clutter); Close calls section + Phase 15 The Interrogation (the Closer grabs you, creepy voice "Where are the others?", 8 s timer, name a friend and it vents to them, refuse and you're taken) + Phase 16 more close calls. Old Phase 14 (full test) is now 17.

## 2026-11-10 (Phase 2: shared chores, built in Studio)
- ChoreService: no owners (assignTo nil), tools one copy per crew member, `touch()` stamps ActiveBy/ActiveById/ActiveAt on every way of working a chore, finishing clears it and credits the Chores stat to that player, `shrinkFor` replaces Reassign/RemovePlayer's owner logic (list shrinks when someone is lost), Reclaim is a no-op, AddSpill unowned. HudController shows "· Sam is on it" (30 s, re-checked every 5 s). WingService/AfkService messages: "{n} chores came off the list".
- Tested in a Studio playtest through the MCP (solo): no owners, no owner-locked prompts, 1 mop for a crew of 1; mopping stamps ActiveBy and finishing clears it; a fake "Sam" on the graffiti chore shows "· Sam is on it" on the HUD and it expires after 30 s; no new console errors. Not testable solo: 2+ tool copies and the list shrinking when someone is lost.
- **Found: Git is ~3 weeks behind Studio.** The repo (`Documents/GitHub/8-Wings-Til-Dawn`, last commit 2309bf2 on 2026-10-21) has 82 scripts / 0.92M chars; Studio has 133 scripts / 1.9M chars, and only ~20 files match. Everything since 2026-10-22 (shop, after-death, shift flow, crew, AFK, titles, gadgets, offers, dawn, etc.) exists only in the place file. **A Rojo sync now would wipe it.** Phase 2 is not committed for this reason; plan: Ethan saves a .rbxlx copy into the project folder, Claude exports every script from it to `src`, checks every hash against Studio, and commits once.
- Backups of the four edited scripts' old sources: `ServerStorage.__Phase2Backup` (remove after the export).

## 2026-11-10 (redesign plan)
- Ethan: the chore loop is boring, wings feel the same (especially over 8), eye height feels low, taken players can't find friends among ~40 cameras. Researched Doors, Dandy's World, Lethal Company, R.E.P.O. and Among Us; reviewed ChoreService, AfterDeathService, WingService, WingBuilder, SpectateController, Movement and Config in Studio.
- Wrote [[Design/Redesign Plan]]: 12 phases, each self-contained. Ethan's calls: all chores shared; one School Rule per wing; ID badge revive (carry it out → back next wing); keep cameras but add a friend-follow spectate; fix eye height. Claude's defaults (Ethan can override): 5 wings, raise the camera (`Config.Characters.EyeLift`) instead of rescaling, badge left behind = out for the rest of the run, full camera network → Security Desk in the main office.
- Found: every taken player is currently respawned free at each new wing (`WingService.buildWing`), so being taken has almost no cost. Phase 6 changes that.
- Open for Ethan: new name, badge penalty, teach rules per run or per account, The New Hire's lunge.
- Ethan's answers (same day): name "5 Wings Until Dawn"; nobody can pick up the wing key while a badge is on the floor (badges stack in one hotbar slot so a lone survivor can still carry the key); teach rules the Doors way (no explanations, a death-screen hint on every death that gets more exact the more often that rule took you). Still open: The New Hire's lunge (default: no kill, sanity −30 + the Closer hunts you).
- Later the same day: a fairer late-badge rule, **Lost & Found** (a badge left behind turns up in the next wing's Lost & Found bin, blocks that wing's key, and picking it up brings its owner back at once). The New Hire doesn't kill (jumpscare, sanity −30, the Closer hunts you). Rule deaths add a journal page with a "Journal updated" cue. New phases: 12 = batteries (charger gives one per player present, more findable batteries, shake a dead flashlight for weak light, slightly stronger eye-adjust), 13 = gloved first-person viewmodel hands holding the flashlight, journal and items. The old Phase 12 (full-run test) is now 14.

## 2026-11-05 (lighting question)
- Found: Studio's `Config.Lighting` has the 2026-10-28 hallway settings (`HallPoolAngle/Range/Brightness`, `HallFill*`), but Git's `src/shared/Config.luau` and `LightFx.luau` don't (still `HallLightScale`). Export them to `src` before any Rojo sync or the sync wipes the hallway lighting.
- Floor circle size is set by Range, not Angle: the Pool SpotLight hangs ~11.7 studs up, so the lit circle's radius = sqrt(Range^2 - 11.7^2) (Hall 13 = ~11 across, rooms 14.5 = ~17 across).

## 2026-11-05 (store thumbnails)
- Ethan asked for 4 thumbnails modelled on the top Roblox horror games. Researched the charts, built 4 scenes in Blender with the real Night Custodian and school models, rendered 1920x1080 JPEGs to `marketing/thumbnails/`. Details, upload steps and how to re-render: [[Design/Thumbnails]].
- Ethan still needs to upload them (Creator Dashboard > Places > Thumbnails > Home Page) and activate all four.

---
Older entries: [[Archive/Log 2026-10]].

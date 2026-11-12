# 5 Wings Til Dawn: START HERE

(Formerly *8 Wings Til Dawn*; renamed 2026-11-10, rename in the game happens in Redesign Phase 1.)

## Every session, in this order
1. This page → [[Handoff]] (where things stand and what's next) → the newest entry in [[Log]].
2. [[Systems/Workflow]]: how to work. **Rojo first for code** (edit `src`, Rojo syncs into Studio); **the Studio MCP for everything else** (playtests, checking Studio, models, lighting, assets).
3. The note for the task: a phase in [[Design/Redesign Plan]], or a system note below. Use [[Systems/Code Map]] to find the script before searching.

## Ethan's standing rules (always)
- **Write it down:** decisions, changes, systems, setup and gotchas go in this vault, every time (2026-10-18).
- **Rojo first** for code; the Studio MCP for anything Rojo can't do. No computer use when the MCP can do it (2026-11-12, 2026-10-29).
- **End of every chat: Studio and `src` must match** (2026-11-12). Before the last reply, run the sync check in [[Systems/Workflow]] → End of every chat. Anything changed only in Studio gets exported to `src` and committed; nothing is left only in Studio.
- **Never use low-poly assets.** Only detailed, realistic models from any source (2026-10-19). See [[Systems/Assets]].
- **Toolbox models:** check every import for scripts and strip anything that isn't visual. See [[Systems/Assets]].
- **New .glb from Claude Design / Blender:** run `python tools/glb_colorize.py models/` in the repo first; Ethan imports the `*_roblox.glb` copy.
- **Ethan does 3D imports and asset uploads himself.** Claude prepares the files and says what to upload.
- Lighting must stay **LightingStyle = Realistic**.

## What the game is
Co-op horror, solo to 4 players. Overnight janitors clear a shared chore list in each of **5** school wings before dawn while the Closer hunts them; each wing adds a School Rule to learn. Full plan: [[Design/Redesign Plan]].

## Map of the vault
**Now**
- [[Design/Redesign Plan]]: **the current work**, 18 phases with a tracker. "Work on Phase N" means this note.
- [[Handoff]]: current state, next step, what's waiting on Ethan.
- [[Tasks]]: Ethan's to-dos and things outside the plan.
- [[Log]]: what happened each session, newest first.

**How to work**
- [[Systems/Workflow]]: session checklist, Rojo + MCP, playtest recipes, committing, machines.
- [[Systems/Code Map]]: every script, one line each.
- [[Systems/Conventions]]: code style and Studio settings.

**Design** (what the game should be)
- [[Design/Decisions]]: every settled decision (the redesign overrides older lines).
- [[Design/Core Loop]] · [[Design/Setting]] · [[Design/The Closer]] · [[Design/Monsters]] (the five rule monsters) · [[Design/Scares]] · [[Design/After Death]] · [[Design/Thumbnails]] · [[Design/Objective Brainstorm]] (history)

**Systems** (how it's built now)
- [[Systems/Chores]] · [[Systems/Inventory]] · [[Systems/Map]] · [[Systems/Lighting]] · [[Systems/UI]] · [[Systems/Music]] · [[Systems/Voice]] · [[Systems/Assets]] · [[Systems/Night Custodian Model]] · [[Systems/Performance]]

**Archive** (history; check before trusting)
- [[Archive/Log 2026-10]] · [[Archive/Tasks pre-redesign]] · [[Archive/Handoff 2026-10-21]]

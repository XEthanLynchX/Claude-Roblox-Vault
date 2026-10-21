# 8 Wings Til Dawn: Index

Start here. One line per area; details live in the linked notes.
New conversation? Read [[Handoff]] first: current state, how Claude works on the project, next steps.

## Rules for Claude (always follow)
- **Open this vault at the start of every session** (Ethan's rule, 2026-10-19): read this index and [[Handoff]] before doing any work on the project.
- **Write it down:** anything relevant for future conversations (decisions, changes, systems, setup, gotchas) goes into this vault, every time (Ethan's standing rule, 2026-10-18).
- **Never use low-poly assets.** When inserting or sourcing any object for the game (Creator Store, Toolbox, generated meshes), only use detailed, realistic models. No low-poly, blocky or stylized-simple assets, ever (Ethan's rule, 2026-10-19). See [[Systems/Assets]] → Where art comes from.
- **Toolbox models:** check every imported model for scripts before using it; strip anything that isn't visual. See [[Systems/Assets]] → Toolbox safety.
- **New 3D model (.glb) from Claude Design?** Before Ethan imports it, run `python tools/glb_colorize.py models/` in `Documents/8 Wings Til Dawn`. It bakes material colors into a texture so Roblox doesn't import it white. Ethan imports the `*_roblox.glb` copy. Details: [[Systems/Assets]].
- Lighting must be **LightingStyle = Realistic** (newer Studio has no Technology/Future setting).
- Code changes: compile-check with luau-compile (not installed on the linked computer as of 2026-10-18; a Studio playtest with no errors is the check), then git commit in the project folder. Git from Claude's shell can leave `.lock` files in `.git`: move them aside (`mv .git/HEAD.lock .git/stale-HEAD.lock`) or Ethan's git will fail with "File exists".

## Concept
- **Pitch:** Co-op horror (solo to 4 players). Overnight janitors finish their chore list in each of 8 school wings before dawn, while the Closer hunts them and the school scares each of them differently.
- **Genre:** Co-op objective horror; private scares create doubt between friends
- **Core loop:** Read the chore list → split up and work → get the wing key → exit together → next of 8 wings

## Design
- [[Design/Decisions]]: every settled decision (rules, scope, money, launch). Read this first.
- [[Systems/Chores]]: how every chore plays now
- [[Design/Core Loop]]: chores, key, pressure, sanity
- [[Design/Objective Brainstorm]]: why chores became the main objective
- [[Design/Scares]]: private scares (formerly anomalies)
- [[Design/Setting]]: the school, wings and mood
- [[Design/The Closer]]: the monster, its behavior and the kill jumpscare
- [[Design/After Death]]: PLANNED (design settled), not built: taken players spectate through the CCTV cameras and can scare their friends every so often
- [[Design/Monsters]]: 8 signature monsters, one per wing (rules, counterplay, model briefs)

## Systems
- [[Systems/Conventions]]
- [[Systems/Night Custodian Model]]: the Blender model, rig and 4 animations of the Night Custodian
- [[Systems/UI]]: the UI look (palette, task screens, HUD gauges) and UiTheme
- [[Systems/Inventory]]: the 3-slot hotbar (items, flashlight slot, drop)
- [[Systems/Map]]: generated grid hallways (wing layout rules) and the baked school surfaces
- [[Systems/Assets]]: how to swap in real 3D models (asset slot names, placement rules)
- [[Systems/Music]]: the score layers, the PA waltz from the ceiling speakers, the HuntDread chase
- [[Systems/Voice]]: every voice line, the three voice packs, normal vs creepy voice
- [[Systems/Lighting]]: ceiling light pools, flashlight beam, join fade
- [[Systems/Performance]]: measuring FPS, shadow budget, prop lightening, heavy assets

## Tracking
- [[Tasks]] · [[Log]]

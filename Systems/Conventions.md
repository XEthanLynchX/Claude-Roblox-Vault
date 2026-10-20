# Code Conventions

- **Language:** Luau, `--!strict` where practical
- **Layout:** `ServerScriptService` (server logic), `StarterPlayerScripts` (client), `ReplicatedStorage/Shared` (modules both use)
- **Remotes:** live in `ReplicatedStorage/Remotes`; the server validates everything the client sends
- **Naming:** PascalCase modules and services, camelCase locals
- **Data:** one DataStore module owns saving and loading
- **Workflow:** Rojo + VS Code + Git; Rojo is already connected to Studio
- **Studio setting (required):** Lighting → **LightingStyle = Realistic**, PrioritizeLightingQuality on (newer Studio replaced Technology/Future with these). Scripts can't set it.
- **Importing models:** always run `tools/glb_colorize.py` on new GLBs first, then import the `*_roblox.glb` copy (see [[Systems/Assets]]).
- **Ceiling lights:** each fixture = PointLight (master, fill) + SpotLight "Pool" (floor circle), see [[Systems/Lighting]]. Never set their properties directly. The server only sets `ReplicatedStorage.PowerOn`; each client's `Shared/LightFx` renders the lights. Effects use `LightFx.push(light, "off"|"red")` and call the release function when done.
- **Streaming is on:** clients don't have the whole map at once. Any client code that collects parts (lights, doors, markers) must also listen for them streaming in (DescendantAdded), not scan once.
- **Edit-mode testing:** Studio caches required ModuleScripts in edit mode, so after Rojo syncs a change, a `require` from the command bar/plugin can return the OLD module. Build previews from fresh clones (copy Props + Materials into a temp folder and point a cloned WingBuilder at it), or test in Play mode.
- **Team Create + Rojo:** only one person can sync at a time (Rojo's sync lock). The other pushes code through Git; the syncing person pulls.

## Rojo sync rule (2026-10-19)
- Rojo makes Studio match the files in `src`. Any script edited only in Studio is overwritten by the next sync. That is what "reverted" the night session's work once.
- Claude edits scripts in Studio (through the MCP tools), so **at the end of every change Claude copies the changed scripts back into `src`, checks that every script's hash matches, and commits.** Only then is a sync safe.
- Before Ethan syncs: ask "is src up to date?" or check that `git status` is clean and the newest commit has the work.

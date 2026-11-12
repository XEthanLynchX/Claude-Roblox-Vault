# Handoff

Where things stand right now. Update the **Now** section at the end of every session. How to work: [[Systems/Workflow]].

## Now (2026-11-12)
- **Working through [[Design/Redesign Plan]].** Done: Phase 2 (shared chores; built and tested solo, the 2-player test is still to do). **Next: Phase 12 (batteries)**, then 14 → 3 → 13 → 1 → 6 → 4 → 5 → 7 → 8 → 9 → 15 → 16 → 10 → 11 → 17 → 18.
- **Git:** `src` was re-exported from Studio in full on 2026-11-10 (commit `a02b0e2`, includes Phase 2). Ethan was asked to push it; check `git log origin/main` before relying on it. From now on code is written in `src` with Rojo syncing (Rojo first).
- **Before starting a phase:** game repo folder connected, `rojo serve` running with the Studio plugin connected, Studio open (Edit mode).
- **Before ending any chat:** Studio and `src` must match, and the result is committed ([[Systems/Workflow]] → End of every chat).

## Waiting on Ethan
- Push `a02b0e2` to GitHub (if not done).
- A 2-player playtest of shared chores (extra tool copies, list shrinking when someone is taken or leaves).
- New voice lines when phases produce them (uploads are Ethan's).
- Thumbnails: Ethan picks them at launch.

## Where things live
- **Repo:** `8-Wings-Til-Dawn` (GitHub `XEthanLynchX/8-Wings-Til-Dawn`), paths per machine in [[Systems/Workflow]] → Machines. Rojo maps `src/shared`, `src/server`, `src/client` (and `src/lobby/*` via `lobby.project.json`).
- **The map is built at runtime** by `WingBuilder`; nothing in Workspace is the map.
- **Only in the place file (save/publish to keep):** `ReplicatedStorage.Assets` (3D models), `ServerStorage.Closer` and other models, `ServerStorage.AssetBackups`, `ServerStorage.__Phase2Backup` (pre-Phase-2 sources; delete after the 2-player test), Lighting properties, MaterialService `School*` variants.

## Known issues (not yet in a phase)
- None beyond the Redesign Plan. Put new ones under the plan's "Found while building" or here.

Older handoff (2026-10-21): [[Archive/Handoff 2026-10-21]].

# Map: generated wings and school surfaces

Built by `src/server/Services/WingBuilder.luau` (2026-10-21, commit `a58c817`). Every wing is a generated school wing; the library builder is switched off (`Config.Library.Wings = {}`, code kept; put `{ 2 }` back to bring it back).

## Hallway layout (tryLayout / layout)
- Junctions on a 4 x 4 grid, `PITCH = 84` studs apart (72 floor tiles, so tile lines meet the walls). Hallway segments (70 long, 14 wide, 12 high) join neighbouring junctions.
- Entrance hall (`STUB_LENGTH = 33`) from the start wall (z = 0, welcome banner, notice, plaque) to the first junction.
- A random tree of 10-12 junctions grown from the entrance, then `GRID_LOOPS = 2` extra links = loops. A layout is rejected (and retried) if it has fewer than 2 dead ends, a dead end at the end of a blind corridor longer than 2 segments, or the exit closer than 3 segments from the start.
- Exit doors: the dead end farthest from the start. Chained gym doors: the next dead end.
- Junctions: floor, ceiling, a wall on every side no hall leaves from, 4 corner posts. Trash cans in corners between two walls (max 5).
- Straight runs of hallway are named for chores/cameras: "the main hall" (entrance + its column), rows "the front / middle / back / far back hall", columns "the west / east / far west / far east / north hall" ("the other ..." for repeats).
- Security cameras: one per straight run (usually 6-9), at its start. CCTV chore picks from these.
- Vents along every segment; entry points = exit, gym, vents.
- Sizes from 300 test seeds: ~12 segments, ~28 rooms (~24 classrooms) per wing; the old wing had ~18 rooms. ~26-29k parts, builds in under 1 s.

## Rooms
- Two slots per side of each segment (z = 17 and 53), one per side in the entrance hall. A slot becomes a room only where its footprint doesn't overlap a hall, junction or another room (rooms back onto each other between parallel halls); otherwise it's wall dressing (lockers, vending, trophy case...).
- First four rooms placed: custodian room, boys', girls', main office; the rest 65% classroom.
- Windows only when nothing is behind the room's outside wall.
- Rooms have a plaster face (`DoorWallFace`) on the inside of the door wall, so the hallway's block wall doesn't show inside classrooms.

## School surfaces (shared/Surfaces.luau)
- From Ethan's `models/school-surfaces.glb` (too heavy to tile: ceiling tile alone is 144k triangles). `tools/bake_surfaces.py` (Blender as a Python module, `pip install bpy`) renders each surface to colour / normal / roughness maps in `textures/`.
- Uploaded by importing `models/school_surface_swatches.glb` (one plane per surface) with Import 3D; the texture ids are in `Config.Surfaces`.
- **Scripts can't set MaterialVariant maps**, so the variants (`SchoolHallVCT`, `SchoolHallBorder`, `SchoolHallCMU`, `SchoolPopcorn`, `SchoolBathFloor`, `SchoolBathWall`) live in MaterialService in the place file. `require(ReplicatedStorage.Shared.Surfaces).ensure()` from the command bar / Studio MCP (edit mode) recreates them from Config. `Surfaces.apply(part, name, tint?)` does nothing if a variant is missing (old plain look).
- Used on: hall floors (VCT, navy border strip), hall walls / end walls / posts / door headers (block wall + oak chair rail at 3.5 + navy rubber base), hall and bathroom ceilings (popcorn), bathroom floor and wall tile (white glazed tile + a band in the room colour). Classrooms unchanged.
- **Classrooms (2026-10-21, from `models/classroom-floor-wall.glb`):** speckled VCT floor with coloured accent tiles (`SchoolClassVCT`, 28 studs per repeat = 24 x 24 tiles), cream painted block on all four walls incl. the inside of the door wall (`SchoolClassCMU`), tan rubber base with a brown cap, popcorn ceiling. Uploaded via `models/classroom_surface_swatches.glb`. Office, custodian room and bathrooms keep their own walls. Themed rooms that lay their own floor (carpet etc.) still show it.
- Bathroom floor overlay is CanCollide false: as a 0.05 step it blocked the Closer's pathfinding into bathrooms.

## Testing notes
- `execute_luau` runs in its own VM: `require` of a service there gives a separate copy (its Dev functions don't reach the running game). Fire the `Dev` remote from the Client context instead (`closer_hunt`, `wing_skip`).
- Verified 2026-10-21: 7 seeds built without errors; pathfinding reaches every hall point and every room doorway; the Closer arrived via a vent, chased across the new layout and took the player.

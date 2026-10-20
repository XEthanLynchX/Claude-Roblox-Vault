# Asset Slots (art pipeline)

Every prop is built by `src/shared/Props.luau`. Before building its own version, it looks for a **Model with the same name** in `ReplicatedStorage/Assets` and uses that instead. Drop a real model in → it replaces that prop everywhere, no code changes.

## Rules for an asset model
- Must be a **Model**, named exactly as below.
- **Pivot** (Model → Edit Pivot) at the **floor** where it stands (or the wall/ceiling surface for wall/ceiling props).
- **Front faces -Z** (the direction the Pivot's front arrow points).
- Anchored parts, no scripts.

## Bookshelves
A custom `Bookshelf` is scaled to the height the room asks for, repeated side by side to fill the width, and placed back to back for the library's double-sided rows.

## Variants
A slot can hold several models: name them `Desk`, `Desk_2`, `Desk_3`... Each placement picks one (by position, so a seed always builds the same room).

## Slot names
| Name | What | Pivot |
|---|---|---|
| `Desk` | student desk + chair (student faces front). Has variants. Optional: child Model `Chair` (pivot on floor under seat, attribute `SeatHeight`) + model attribute `DeskTop` so chairs can be flipped up onto desks at night | floor under desk-top centre, chair on +Z |
| `TeacherDesk` | teacher's desk | floor |
| `LockerBank` | 10 lockers (note: custom banks lose the locker-door scares unless doors are named `LockerDoor`) | floor, back against wall |
| `LightFixture` | ceiling fluorescent (include a PointLight) | ceiling surface |
| `TrashCan` | classroom/hallway trash can | floor |
| `Puddle` | spill decal/mesh (needs a part named `Main`) | floor |
| `ClassroomDoor` | door (hinge at pivot, extends along +Z when closed; needs part `Panel`). Default is built from `models/classroom-door.glb`: oak slab, wired-glass vision lite, lever handles, hinges, kick plates, closer | floor at hinge |
| `DoorFrame` | classroom door frame: jambs, head, stops (default built from `models/classroom-door.glb`) | floor at doorway centre, inside the wall |
| `Whiteboard` | whiteboard | wall surface center |
| `Window` | exterior window | wall surface center |
| `BulletinBoard` | cork board | wall surface center |
| `ExitSign` | EXIT sign | its center |
| `WallClock` | wall clock | wall surface center |
| `WaterFountain` | drinking fountain | floor at wall |
| `Shelf` | custodian shelf | floor |
| `MopSink` | mop sink | floor |
| `TrashChute` | trash chute (needs part `Hatch`) | wall surface |
| `PeriodicTable`, `NoteBoard` | wall art | wall surface |
| `BookStack` (+ `_2`, `_3`) | books lying flat; teacher desks, library tables, lost & found | surface |
| `BookRow` | short row of standing books; top of classroom shelves | surface |
| `PencilCup` (+ `_2`) | pencil cup on teacher desks | surface |
| `BoardEraser` | eraser on the whiteboard tray | surface |
| `Poster` (+ `_2`.. `_5`) | wall posters (from dawnridge-wall-art.glb) | wall surface, centre |
| `WelcomeBanner` | "Welcome to Dawnridge High" banner on the main hall start wall | wall surface, centre |
| `Easel` | easel with its canvas (one model, canvas side = front) | floor |
| `Flask` (+ `_2`.. `_5`) | lab flasks on science benches (evenly spaced, never overlapping) | surface |
| `Backpack` (+ `_2`.. `_6`) | backpacks: room clutter, lost & found table | floor |
| `TrophyCase`, `VendingMachine`, `Bench`, `Bookshelf`, `Piano`, `Drum`, `Easel`, `LabBench`, `ComputerStation`, `Table`, `Chair`, `Stool`, `Globe`, `Flag`, `Plant`, `MusicStand` | furniture | floor |
| `FireExtinguisher`, `BreakerPanel`, `SanityBoard` | wall fixtures | wall surface |
| `Banner` | ceiling banner | its center |
| `WallClock` | Ethan's wall clock. Hands `HourHand`/`MinuteHand`/`SecondHand` with attribute `Rest` (12:00 pose, pivot space); model attribute `Hub`. Clients turn the hands (ChoreFx) | wall surface, front -Z |
| `StepLadder` | twin-step ladder for the co-op light chore, attribute `TopHeight` (6.9) | floor, climb axis Z |
| `DeskPhone`, `CrtMonitor` | office front desk phone, security monitors (`Front` part = screen, attribute `Screen`) | desk surface |
| `SecurityCamera` | CCTV camera at the start of each hall (lens looks -Z) | its centre |
| `Broom`, `Vacuum`, `Plunger`, `BatteryBox`, `BulbBox` | hotbar tools / chore props (see [[Systems/Inventory]]) | floor |
| `WetFloorSign`, `Backpack`, `ScatteredPapers`, `PaintSplat`, `JanitorCart`, `BucketStack`, `Ladder`, `WaterHeater` | clutter | floor |

## Importing GLBs with color
Roblox Import 3D only keeps image textures, so models with plain material colors (Claude Design exports) import white.
Run `python tools/glb_colorize.py models/` in the project folder: every `.glb` gets a `*_roblox.glb` copy with its colors baked into one small texture. Import that copy.

## Where art comes from
- **Claude Design** (decided 2026-10-16, replaces Blender): all visual design work is done as Claude Design artifacts: concept art and reference sheets for props and the Closer, UI (HUD, clipboard, shop, lobby screens), the game icon/thumbnail, decals and textures (posters, signs, notes, the wellness monitor).
  - Claude Design produces **2D** designs. It can't output 3D meshes, so 3D props still need a mesh source: the Design reference sheets are the spec, and the meshes come from the Creator Store (or a modeler working from those sheets).
- **Creator Store** (Toolbox → Creator Store → Models): search "school desk", "locker", etc. Prefer **mesh** models, check they have no scripts. Match them to the Claude Design reference sheets.
- **NEVER low-poly** (Ethan's rule, 2026-10-19): never insert low-poly, blocky or simple-stylized assets. Only detailed, realistic models. If the only options are low-poly, say so and ask Ethan instead of inserting one.
- ~~Blender~~: no longer used for general props. Exception (2026-10-19): the Night Custodian character was built in Blender; see [[Systems/Night Custodian Model]].
- Note: Rojo doesn't sync `ReplicatedStorage/Assets`, so assets live in the Studio place file. Save the place regularly.

## Imported models (meters!)
Import 3D brings .glb files in at 1 metre = 1 stud, so everything comes in tiny. Claude rescales on setup.
The door and frame assets are rebuilt part by part from `classroom-door_roblox.glb`: the slab is stretched to the 6-stud doorway, hardware keeps its proportions. Parts tagged `Sided` (hinges, closer) move to the other face on doors that swing the other way.

## Placement rules
- Trash cans: classrooms in the corner by the door wall; hallways only in the corners at the ends of the main and back halls, never next to a doorway.
- Radios / record players stand against a room wall, at a spot checked to be empty (no desks, shelves or doors).
- Random room clutter (papers, backpacks, tipped chairs) stays out of the area just inside the door.
- Classroom doorways are 4.6 studs wide; the Closer's path agent (radius 2) still fits past an open door.

## Toolbox safety
- 2026-10-19 afternoon: Creator Store desk phones, CRT monitors, a filing cabinet and a CCTV camera listed as "No scripts" ALL carried hidden scripts (`Runtime`, `ScreenBuilder`, fake `LightConfig`/`EasyConfiguration`/`CoreTextureSystem` loaders). The listing text is not proof: always scan.
- 2026-10-19: the MopBucket import carried the same `Runtime` backdoor (inside a part named `Hair`). Scan every import for scripts named `Runtime` / `ScreenBuilder` or containing `runtime-object-defaults`.
Check every Toolbox model for scripts before using it. On 2026-10-18 the canvas, easel and two paintings carried a hidden Studio-only "Runtime" script that shows a fake "Roblox Studio" window asking you to copy a command: a known backdoor scam. Removed. Never paste code from a popup like that.


## Variant weights (2026-10-18)
- Any asset model can have a `SpawnWeight` number attribute (default 1). 0.3 = shows up about a third as often. Desk_2 = 0.3 (heavy model).

## Flashlight (2026-10-18)
- `Assets.Flashlight`: lens end faces -Z, lens part named `Lens` (glows Neon when on). Scripts scale it: 0.55 studs in first person, 1 stud in hands. Code: src/shared/FlashlightModel.luau. Without the asset, a plain cylinder is used.
- ServerStorage.AssetBackups holds originals of edited assets (Bookshelf_2_Original, Flashlight_Import).

- **ExitDoors** (2026-10-20): `models/school_exit_doors_roblox.glb` (colors baked by `tools/glb_colorize.py`). Only the double door is used: left door closed, scaled to 8.4 tall, pivot = floor at the back of the frame, front -Z (panic bars face the hall). WingBuilder hides the ExitDoor part (keeps prompt/sounds/collision) and welds the model to it so the arrival rattle still shakes it. Full import (single + double) kept in AssetBackups.

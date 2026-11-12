# Store thumbnails

Made 2026-11-05 in Blender. Four 1920×1080 JPEGs (all well under Roblox's 3 MB home-page limit) in the project folder `marketing/thumbnails/`:

| File | Idea | Tagline |
|---|---|---|
| `thumbnail_1_chase.jpg` | The Closer lunging down a locker hallway at a fleeing janitor | none |
| `thumbnail_2_closeup.jpg` | Close-up of its skull grin and red eyes, head cocked | SURVIVE THE NIGHT SHIFT |
| `thumbnail_3_flashlight.jpg` | First-person: the player's hand + flashlight, the Closer standing in the beam at the end of a dark hall | DON'T SPLIT UP |
| `thumbnail_4_coop.jpg` | Three janitors working in a classroom, unaware; the Closer watching from the doorway | 1-4 PLAYER CO-OP |

## Why these (research on the Roblox charts, 2026-10-29)
- Top horror games then: The Hollow (event, ~194K playing), 99 Nights, DONT SPLASH, Dandy's World, Violence District, DOORS, Forsaken, Scary Shawarma Kiosk.
- Patterns: one monster big and lit against a dark/red background (DOORS, The Mimic, Survive the Killer); cinematic realistic lighting with the monster down a hallway or in a flashlight beam (DONT SPLASH, PERIPHERAL, FATHOM); a story moment (a player fleeing; danger the characters can't see).
- Roblox guidance: 16:9, 1920×1080, keep text and important things away from the bottom (player-count overlay), text sparingly.

## Uploading
- Creator Dashboard > the game > Configurations > Places > the place > Thumbnails > Home Page tab > Edit active thumbnails. Upload all four and activate them. With 2–5 active, Roblox's thumbnail personalization shows each to a random set of players and favours the winner per group. Leave them all active until the next big update.

## The Blender file
- `marketing/thumbnails/Thumbnails.blend`: one scene per thumbnail (`T1_Chase`, `T2_Face`, `T3_Hall`, `T4_Coop`). Real game models: the Night Custodian (copied rig from `models/NightCustodian.blend`), lockers, classroom/exit doors, desks, flashlight, journal, wall art, the baked hall/classroom surfaces. Player avatars are simple Roblox-style stand-ins built in Blender (not game assets).
- Everything is scripted. Text block `thumb_lib` = helpers (hallway/classroom builder, avatar, pose aiming, auto-framing, logo, compositor). `tb_T1`..`tb_T4` = one builder per scene. To change one: edit its `tb_Tn` text, run it, then run `exec(bpy.data.texts['thumb_lib'].as_string()); final('T1_Chase','thumbnail_1_chase.jpg')` (scene names above).
- Look: EEVEE, AgX Punchy, world volume fog, compositor bloom + vignette + contrast + slight chromatic aberration. Logo = Impact text (red "8", bone white, red rule) parented to the camera with a soft dark scrim behind it.
- Draft renders are in `marketing/thumbnails/_previews/`.

## Gotchas (Blender MCP)
- The Blender screenshot tool often shows a stale image-editor frame. To check a render: grant the `marketing` folder, stage the file with device_stage_files and look at it.
- `bpy.ops.wm.redraw_timer` hung the bridge once: don't use it.
- Light `volume_factor` keeps rim/key lights from washing the fog out; use it.
- Arms reaching toward a low camera look raised (perspective): pose them down toward the target or raise the camera.

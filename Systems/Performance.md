# Performance

## How to measure
- Studio playtests run server + client + editor in one process, so FPS there is low regardless of the map (2026-10-18: ~34 FPS with the wing deleted). Test FPS in the real Roblox player.
- In the player: Shift+F5 (frame stats), Ctrl+F6 (microprofiler). Settings > Graphics Quality manual 10 for worst case.

## What's in place
- **Shadow budget** (LightFx): only the `ShadowLights` (5) nearest ceiling pools cast shadows; recomputed every 0.2 s. Low mode: none.
- **Light culling**: fixture lights further than `LightCullDistance` (130) are disabled; the neon panel stays lit so halls still look lit far away.
- **lightenProps** (WingBuilder, run on Shell + Decor before parenting): CanTouch off everywhere (no game code uses Touched); CastShadow off for parts with largest side < 1.6 or thin (< 0.3) and < 4 long; CanCollide off when largest side < 1. CanQuery left on (MusicPlayerService placement check and raycasts need it).
- Both values tunable live via `ReplicatedStorage.LightingTuning`.

## Known heavy content
- Desk_2: 112 textured mesh parts each; SpawnWeight 0.3 so ~5 per wing. Merging would drop its texture (unions don't keep MeshPart textures); a simplified re-export from Blender is the real fix.
- Bookshelf_2: merged to 3 unions (was 172 parts). Backup in ServerStorage.AssetBackups. Merge trick for flat-colored Part assets: GeometryService:UnionAsync per material, UsePartColor false keeps colors.
- Bookshelf: 40 book decals.
- Wing ≈ 19k parts, ~75k instances, ~3.5 GB Studio memory.
- Ideas not done yet: merge/simplify Desk_2 and Bookshelf_2 (Blender, fewer meshes), lower their spawn weight, StreamingEnabled is already on.

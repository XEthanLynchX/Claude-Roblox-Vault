# Lighting

## How to tune it yourself (updated 2026-10-18)
**The scene** (how dark it is, moonlight, exposure, fog, glow, color grade) is set directly in Studio and saved with the place:
- `Lighting`: Ambient (darkest indoor level), OutdoorAmbient, Brightness (moonlight), ExposureCompensation (overall brightness), ClockTime, ColorShift, EnvironmentDiffuse/SpecularScale
- Children: `Atmosphere` (fog), `Bloom`, `NightGrade` (tint, contrast, saturation), `DarkGamma` (lifts the darkest shadows)
- No script overwrites these. Blackouts dim from whatever Lighting is set to and restore it after (live edits during a playtest survive a blackout).
- These live in the place file, not Git: share them through Team Create / saving the place.

**The lights themselves** (ceiling pools, fixture glow, flashlight beam, blackout darkness, join fade) are in `Config.Lighting` in `src/shared/Config.luau`:
- Live: playtest → Explorer (Client view) → `ReplicatedStorage > LightingTuning` → edit attributes; instant.
- Keep: tick `PrintForConfig`, paste the printed lines into `Config.Lighting`.
- Keep `FillRange` under ~9 (shadowless, shines through walls).

## Low graphics mode (2026-10-18)
- **Cause of the flat look:** below graphics quality **6** (player slider; Studio QualityLevel 13 of 21) Roblox draws no light shadows and lights the world in coarse 4-stud voxels. Wide shadowless pools flood everything and the flashlight (shadowed lights) vanishes. Confirmed: Ethan's game looked right once quality was set to max.
- **Fix:** LightFx picks a mode every 3 s:
  - Manual graphics below `LowModeBelowQuality` (6) → Low.
  - Automatic graphics running under `LowModeFps` (35) for two checks → Low (never switches back by itself, since low mode raises FPS).
  - `ForceLightingMode` = "High"/"Low" forces it (testing; live in LightingTuning).
- **Low mode:** pools 60°, range 13, brightness 1.8, no shadows, no fill glow; flashlight 50° / 6, rim 70° / 1.5, throw 20° / 4, no shadows. Result: dark, moody hall with dim pools; flashlight is a warm wash (a crisp circle is impossible without shadows).
- Values are the `Low*` keys in `Config.Lighting`. Any value without a Low version is shared.
- Studio prints `[LightFx] lighting mode: LOW/HIGH (reason)` when it switches.
- To look at low quality in Studio's edit view: `settings().Rendering.EditQualityLevel = Enum.QualityLevel.Level05` (restore to Automatic after). Claude's screenshots only work in Edit mode.

## Current look (2026-10-18, matched to Ethan's reference screenshots)
- Ceiling pools: Angle 120, Range 14.5, Brightness 3.5 (fill 0.3). The short **range** is what makes each fixture a distinct circle on the floor with dark gaps; the wide angle puts arches of light on the walls. Lower PoolRange = smaller pools.
- Flashlight: FlashAngle 28 / Brightness 4, rim 31 / 0.15, throw 10 / 3 → crisp bright circle, hot center, dark right outside it.
- The section below has the original (narrower) numbers for history.

## Ceiling lights (2026-10-18)
- Each fixture has two lights on its `Diffuser` part:
  - **Pool** (SpotLight, Face = Bottom, Angle 70, Range 18, Brightness 2.6 / 1 when weak, Shadows on): lays a soft circle of light on the floor under the fixture. Spaces between fixtures stay dark.
  - **Fill** (PointLight, Range 9, Brightness 0.25 / 0.1, Shadows off): faint glow around the fixture. Kept short so it can't light through walls (shadowless lights pass through walls).
- The PointLight is the "master": the server, scares and LightFx all work with PointLights. LightFx switches the Pool SpotLight to match its fixture (on/off/red). Never set fixture lights directly; use `LightFx.push`.
- Hanging banners don't cast shadows (they cut hard dark bars through the pools).

## Loading in
- Roblox takes a moment to draw many shadow-casting lights after you join, so lights seemed to "come on slowly". The HUD's fade frame now starts black and fades in (1.2 s) 2 s after the wing and your character are loaded, so you never see them pop on.

## Flashlight (2026-10-18)
- Round beam like the reference images (dark everywhere except a soft circle):
  - **Hotspot** SpotLight: Angle 30, Range 60, Brightness 2.6 × charge, Shadows on
  - **Spill** SpotLight: Angle 44, Brightness 0.3, no shadows, range clamped to just past what you face (soft edge of the circle)
  - **Throw** SpotLight: Angle 11, Range 60, Brightness 2.2 × charge (carries down long halls)
- Previously the spill was 70° wide, which made the beam look like an outward fan.
- New flashlight model: `models/black-flashlight.glb` → colorized copy `black-flashlight_roblox.glb` (ready to import; not wired in yet). The first-person and in-hand flashlights are still the simple built-in ones.

## Scene settings (saved in the place's Lighting, as of 2026-10-18)
ClockTime 23.2, Brightness 0.25 (moonlight), Ambient (13,14,20), OutdoorAmbient (7,8,12), ExposureCompensation −0.4, Atmosphere density 0.32. Client adds a small gamma lift (`DarkGamma`). Blackouts dim the scene further (LightFx).

## Previewing lighting in Studio
Edit mode uses Studio's daylight. To preview: temporarily copy the scene settings above onto Lighting, build a wing, screenshot, then restore Lighting. (Required modules are cached in edit mode; build from fresh clones of Props/WingBuilder.)


## Shadow budget (2026-10-18)
- Only the nearest `ShadowLights` (5) pools cast shadows; lights past `LightCullDistance` (130) are off. See [[Systems/Performance]].
- Low-mode flashlight: LowFlashBrightness 3, LowFlashEdgeBrightness 0.6, LowFlashThrowBrightness 2.

## Materials (2026-10-20)
- MaterialService base-material overrides: CeramicTiles -> GlossyVinylTile1, Plaster -> GlossyPaintedPlaster (generated with the Studio assistant, under MaterialService.AssistantMaterials). Every tile floor / plaster wall uses them without code changes. To undo: MaterialService:SetBaseMaterialOverride(material, "").
- Lighting.EnvironmentSpecularScale = 1 (was 0.2) for the sheen; EnvironmentDiffuseScale = 0.25.

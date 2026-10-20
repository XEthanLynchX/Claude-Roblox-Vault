# UI look (theme)

Set 2026-10-19. One look for every screen: worn steel plates, bone-white typewriter text,
dried-blood red for danger, sodium-lamp amber for things you can press.
Code: `src/client/Controllers/UiTheme.luau` (colors, fonts, tween timings, helpers). Use it for any new UI.

## Palette (UiTheme.Colors)
- Void 6,6,7 (backdrops) · Plate 19,20,20 (panels) · PlateDark 11,11,12 (headers, wells) · Edge 64,61,55 (outlines)
- Bone 214,204,182 (text) · BoneDim 150,143,128 · Faint 92,88,80 (hints)
- Blood 148,26,22 · BloodBright 205,46,36 (danger, stamps) · Amber 205,156,72 (buttons, selection) · Glow 236,222,182 (battery cells)
- Fonts: SpecialElite (typewriter) for titles/body/buttons, GothamMedium for tiny labels, Code for numbers, GothamBlack for stamps.

## Task screens (Minigames.luau)
- The world dims behind the task (backdrop + vignette): you're busy and exposed.
- Panel = a "work order" steel plate: dark header strip with a red tab, the task name, a random work-order number (W.O. ####), a thin red rule, rivets in the corners, light falling from the top, faint film grain, and the lights dip now and then.
- Buttons: dark with a coloured outline (amber by default, red for danger), hover lights up, press dips.
- Success: a red rubber stamp "DONE" slams onto the plate (scale 2.3 -> 1, thud, small shake), then the panel shrinks away.
- Failure: red flash + jolt, then a typewriter message mid-screen with a red underline.
- Responsive: the 560x420 design sits in a "Holder" with a UIScale fitted to the screen (UiTheme.fitToScreen). The camera feed (Monitor) is full screen and skips the plate styling.

## HUD gauges (HudController.luau)
- **Breath (stamina):** thin line under the hotbar that burns down from both ends; hidden 1.2 s after you're fully rested; shifts to red below 30%; "OUT OF BREATH" pulsing red when exhausted.
- **Flashlight battery:** battery icon bottom right (5 cells, nub), spare batteries as small pips beside it (up to MaxSpareBatteries). Low (<20%): cells red, outline blinks. Empty: DEAD blinks in the casing. "[R] SWAP BATTERY" pulses when low and you have a spare. The whole gauge dims (CanvasGroup) while the flashlight is put away.
- **Hotbar:** dark plates, amber outline + amber bar under the selected slot, typewriter item names.
- All three scale with the screen (UiTheme.fitToScreen, designed at 1280x720).

# Music

All original, synthesized with numpy (no samples). Played by `client/Controllers/MusicController.luau`.

## Layers
- **Drone** (`Config.Sounds.Drone`, empty = off), **Tension** (swells as the Closer gets near / menace), from `tools/make_music.py`.
- **Chase** = **HuntDread** (2026-10-21, `tools/make_horror_music.py`, id `90855983320797`): no beat to run to. Fast heartbeat that never slows, an endlessly rising string cluster (Shepard glissando), low brass cluster swelling, irregular heavy impacts with reversed swells, screeching string stabs, ragged breathing, metal scraping. Plays only while the Closer hunts or the Schooler chases. Old chase id `102799446494932`.
- **PA music** = **PAWaltz** (2026-10-21, id `108839993366048`): an original slow D-minor waltz as an old 78 record (pump organ, plucked bass, celesta, wavering theremin lead), wow/flutter, crackle, two tape drags per loop, through a tinny PA horn with hall slapback. The second pass is more worn (octave drops, flat notes, a sour held note).

## PA music behaviour
- WingBuilder puts a round ceiling speaker (`Speakers/PASpeaker`) over every hallway junction plus one in the entrance hall.
- The client plays PAWaltz on every speaker (local Sounds, InverseTapered roll-off 10-75 studs), synced to server time so every speaker is on the same spot; muffled through walls by the same code as the radios.
- Fades out during a hunt (chase takes over), back at 40% while it retreats, dips to 30% while the PA talks (`PA_*` / `Voice_*` sounds), and in a blackout the record winds down (PlaybackSpeed to 0.45) and goes silent until the power is back. Jumpscare Silence mutes it.
- Levels: `Config.PAMusic` (Volume 0.32 per speaker, MinDistance, MaxDistance, RetreatVolume, DuckForVoice).

## Note
- The uploaded files are the preview MP3s (two loops each: 221.5 s and 96 s). MP3 adds a tiny gap at the start, so there may be a faint click at the loop point. `sounds/PAWaltz.ogg` / `sounds/HuntDread.ogg` loop seamlessly; re-upload those if the click is noticeable.

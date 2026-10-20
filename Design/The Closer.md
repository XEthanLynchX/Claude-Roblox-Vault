# The Closer

A calm, methodical night janitor who is tidying up the school, and the players are the mess.
Rule of thumb: players spend 95% of a run *expecting* it and 5% *seeing* it.

## Mostly absent
- Show traces, not the monster: a door that's now open, desks pushed into a line, wet mop tracks, a locker slowly closing.
- Partial sightings only: a silhouette behind a frosted door window, legs under a bathroom stall, a shape at the end of the hall that's gone after the lights flicker.

## Acts like it belongs
- Never runs. It does janitor work: mops, erases chalkboards, locks doors.
- Walks slowly and ignores players until it doesn't.
- **It counts.** Over the PA it quietly counts the people left in the building. The number drops when someone's taken.

## Uses the scare system
- Sometimes visible to only one player, who can't prove it's there.
- Sometimes a hallucination: a low-sanity player panics over nothing, then isn't believed later.
- Disguises itself as a scare, like the "harmless" still student at the desk turning its head.
- **Undoes chores:** re-spills mopped floors, unlocks locked doors, breaks fixed lights.

## Reacts to players
- **Group splits up:** goes after whoever is alone.
- **Someone's loud** (sprinting, slamming lockers): drifts toward that player.
- **Seen too often:** goes quiet for a few loops so players relax.
- **Wing taking too long:** hunts more often and shows itself more.

## Sound
- Familiar sounds in the wrong place: mop bucket wheels squeaking, keys jingling, a lock clicking behind you.
- Quiet and directional, so players must stop talking to locate it.
- Sometimes repeats things players said earlier (chat bubbles or garbled PA audio).

## The kill: two experiences at once
The victim gets the jumpscare; teammates get the silence. Survivors never see what happened.

### Victim's screen (the jumpscare)
1. **Setup (~1–2 s):** sound drops to near silence; only the fluorescent hum stays, then it cuts out too. The victim's controls lock.
2. **Lights out:** a full black screen for about 0.5 s. This is the beat where players brace.
3. **Fake relief:** the lights come back and the hallway looks normal and empty. Hold it for about 1 second.
4. **The hit:** the Closer's face fills the screen in a single frame cut (no slow zoom), with a loud, sharp sound. The camera jolts.
5. **After:** the screen cuts to black, and their own mop clatters on the floor. Then they become a taken player (a ghost anomaly).
- Vary it so it doesn't get predictable: sometimes the face is *behind* the player when the camera is forced to turn, sometimes it's in a classroom window reflection, sometimes it comes from below (under a desk or stall).
- Keep it short (under 3 s) and skippable on repeat deaths in the same session, so it stays scary instead of annoying.

### Teammates' screens (quiet)
- The lights go out for about 2 seconds. No scream, no sound from the victim.
- When the lights return, the player is gone and their mop is lying in the hallway.
- The PA count drops by one.
- A few loops later, their avatar may show up sitting in a classroom.

### Clip value
- Victim streamers get the scream clip; the group gets the "where did he go??" clip. Two clips from one death.

## Scare rules
- Calm before every scare.
- Fake-outs: a locker bangs open on its own and nothing's there.
- Escalate slowly: barely present in early wings, everywhere by the principal's office.
- Never repeat a scare unchanged.

## Implementation notes
- The jumpscare runs on the victim's client only (local camera, local UI and sound), triggered by a server event sent to that player.
- Teammates get a separate "lights out" event; the server removes the victim and drops the mop.

## Implementation (2026-10-16)
- Model: Ethan's `models/the_closer.obj` (hunched janitor: cap, coveralls, name patch, mop, flashlight, radio, grin, halo of floating keys). Imported via Studio Import 3D, named `Closer`, kept in ServerStorage. No skeleton, so it glides as one welded piece.
- Its own flashlight casts a real beam; radio light blinks red; key halo orbits its head (client-side).
- Spawns ~45 s into a wing, far from players. Wanders hallways and rooms at walking pace.
- **Rounds:** every ~1–1.5 min it unlocks a door you locked (chore goes back on the list) or leaves a NEW spill.
- **Hunting:** sees players within ~40 studs (grows over time); in a blackout, a player with their flashlight off is only seen within ~14. Prefers players who are alone. Chase speed 13 (walk 11, sprint 20), faster the longer a wing takes and during blackouts. Loses you after 5 s out of sight.
- Keys jingle near it (audio cue). Being near it in view drains sanity.
- **Kill:** victim gets silence → black → false relief → face + stinger → black. Everyone else: lights out 2 s, silence. Then it vanishes for 25 s.
- Taken players stay down until the next wing (revive purchases come later). Everyone taken → "Everyone was taken" → wing restarts.

## Model + animation update (2026-10-16)
- Switched to `models/the_closer.glb` (has materials/colors; OBJ didn't). No skeleton or animations inside the file, but the parts are organized like one.
- Code builds joints from part names: Waist, Neck, Shoulder L/R, Elbow L/R, Hip L/R (Motor6D).
- Animations are procedural (code, client-side): Walk (stride matches speed), Chase (hunched, arms reaching, head cocked), Idle (breathing, head turns to watch you, sudden twitches), Work (mopping while it undoes a chore), Lunge (the kill).
- Scares that used to show your avatar now show the Closer (idle + head tracking you).

## Per-wing look (2026-10-16)
The Closer manifests more each wing (colors applied in code; Roblox's importer drops GLB colors):
1. Barely There: near-black, 45% see-through, only the eyes glow
2. Faint · 3. Taking Shape (mop appears) · 4. Solid (first keys orbit its head)
5. Keys Gathering · 6. Armed (keyhole in its head glows) · 7. Unlocked
8. Fully Manifested: olive coveralls, porcelain face and forearms, brass gloves/zipper/teeth, full 16-key halo, mop
Dev menu "Look 1–8" previews each stage.

## Menace-meter AI (2026-10-16)
Replaced the fixed timer + coin flips with an "AI director" (Alien: Isolation / L4D style). Code: `MenaceService`, `NoiseService`, `CloserService`. Tuning: `Config.Menace`, `Config.Closer`.
- **Menace 0–100 per wing.** Up: time (0.25/s), each finished chore (+4, also a noise), trash chute (+5), lockers (+2), sprinting (+0.6/s each), players spread out (>60 studs avg), blackouts (+0.8/s), it spotting someone. Later wings fill 18% faster per wing.
- **Out (<30):** not in the building. Faint key jingling from far entry points, PA "night staff count… N…".
- **Arrival (30):** wing lights flicker, exit door rattles, it enters through a fair entry point closest to the most vulnerable player: exit door, chained gym doors, or a ceiling vent (hatches every ~40 studs in both hallways). Never within 40 studs of anyone or in anyone's line of sight; waits if all are covered.
- **Rounds (30–70):** heads to fresh noises, then walks the trail of finished chores (60% chance it messes one up: unlocks a door or leaves a new spill; max 2 per wing). Spots someone far away → stares 2–3.5 s, then backs off. Someone within 14 studs → +20 menace.
- **Hunt (70+ or trigger):** most vulnerable = alone, low sanity, in the dark (in a blackout: flashlight beams). Knows roughly where they are until it sees them; break line of sight for 5–7 s and it searches, then gives up. Triggers: wing key picked up (guaranteed), blackout start, sanity < 25 (90 s cooldown per player).
- **Retreat:** walks out through the furthest entry point (only vanishes unseen), gone 30–60 s (18–36 s by Wing 8; +15 s after a kill). Menace → 35 (30 after a kill).
- **Fairness:** no kills in the first 60 s of a wing or within 20 s of spawning. Key jingle is positional (louder closer). Ceiling lights within 30 studs of it flicker on each client.
- **Later wings:** walk 7→9, chase 13→16.5, sight 40→65 studs, longer hunts.
- **Lockers:** 1–2 lockers per hallway bank are hideable, the ones left slightly ajar. View through vent slits. Same seen-you-hide rule as desks (2.5 s unseen + 22 studs). If it saw you: walks up, pauses, 75% opens it and takes you, otherwise walks past. Getting in/out is a slam (noise).
- Dev menu: status shows menace + phase; buttons Menace 0/30/69/70 and Freeze menace.

## Stalking, music, new jumpscare (2026-10-16)
- **No more walking the halls toward players during Rounds.** It's out of sight (parked) between activities and only moves where nobody can see it. Every few seconds (6–12 s Wing 1, 3–7 s Wing 8) it either messes with a finished chore unseen, switches a radio back on, or **leans around a doorway** to watch someone: body behind the wall, head tilted out, one hand on the frame. It can also lean *into* a classroom from the hallway when you're inside.
  - Peeks from 16–48 studs, never pops into anyone's view as it arrives (it leans out slowly).
  - Stare at it for 1.5 s → it holds your gaze a moment, pulls back and vanishes (+menace).
  - Walk within 14 studs → it comes out of the doorway and **chases you** (after the first minute).
  - Seen standing anywhere else: stares back, walks away from you, vanishes once unseen.
- **Hunts** start from wherever it is; if it's out of sight it bursts out of a classroom or entry point 30–55 studs from the victim. Retreat = back away from viewers, vanish unseen.
- **Music** (all synthesized by `tools/make_music.py`, files in `sounds/`): Drone (always, sinks in blackouts), Tension (swells as it gets close + with menace), Chase (hunt only), all fade out for jumpscares. Diegetic: 2–3 radios / record players in classrooms + a radio in the custodian room playing a detuned music-box lullaby. Players can switch them off; it switches them back on when nobody's near; they slow and warp while it hunts.
- **Jumpscare:** silence → pitch black → false calm of random length (whisper) → 50/50: it's standing in front of you, or it breathes behind you and the camera whips round → lunge into your face with scream + bass hit + stinger, FOV punch, violent shake, red flashes, blur pulses, head snapping, gamepad rumble → strobe + static → black, ringing ears, "you were taken". Victim dies after 5.6 s.

## Polish pass (2026-10-16)
- PA voice live (one packed file, `PAVoice` id). Chime re-uploaded.
- Lock chore can't be done from inside the room; any locked classroom door has an "Open door" prompt on the inside (you step out, it stays locked).
- Radios / record players start silent. First one clicks on after 2–4 min, plays 1.5–2.5 min (or until switched off), then 2–4 min gap before a different one. Only ever one playing. Each client muffles it through walls and with distance (EQ + volume).
- Chore clipboard replaced by a leather shift journal: Tab / J / gamepad Y (touch button on mobile). Objective is written at the top. Small "✎" note bottom-left when it changes. Player list disabled to free Tab.
- Hiding is animated: lockers = step up, door swings open, back in turning to face the vents, door shuts. Desks = crouch in front, crawl backwards under. Exit plays in reverse.

## Walk / run animation (2026-10-16)
- Animated in code (no Blender): the model is rigid parts, and Blender clips would need publishing as Roblox animation assets. Code also allows creepy variation (hitches, twitches).
- Each client shows a smoothed local copy of the Closer ("CloserDouble") following the server's position, so it no longer floats/jumps; the server model is hidden locally.
- Walk: slow hunched limp, right leg drags turned out (toe scraping), hips dip onto it, left arm hangs dead, head cocked, stride freezes for a beat every 3.5–8 s with a head snap. Thud + scrape footsteps.
- Run: bent double, arms clawing forward, head up and jittering, heavy bounce, torso twist, louder footsteps.
- Search: head scans side to side. Ankle joints added (heel strike / toe-off).

## The Night Custodian replaces the old model (2026-10-20)
- **Model:** ServerStorage.Closer is now the skinned Night Custodian (imported `models/NightCustodian_Roblox.fbx`, attribute `Skinned`). Old part model kept as ServerStorage.AssetBackups.OldCustodian_Original. CloserService.findTemplate prefers ServerStorage.Closer; the per-wing looks (CloserLook) only apply to the old model.
- **Animation:** no published animation assets. The 4 Blender clips are baked into `Shared.CloserAnims` (tools/export_closer_anims.py) and played on the bones by `client/Controllers/CloserSkin` (axis + scale fitted from the bone positions at load). Speed-matched playback, 0.3 s blends, head twitch / look-at-you, peek lean, chase freeze-frames.
- **Crawling is per player:** if YOUR sanity is under `Config.Closer.CrawlSanity` (25) you see it crawl (slow) or scuttle (CrawlRun) on all fours; everyone else sees it walk / run.
- **Jumpscare:** its face is shoved into the camera (face placed 1.25 studs away, then 0.8-1 between strobe frames), head tipped up and snapping, arms reaching, eyes burning (emissive x14), red face light + white flashes. HUD hidden during the scare. Its own scream + a bone crack layered on the old scream/hit/stinger.
- **Voice** (Pro Sound Effects, in Config.Sounds): CloserScream1/2 when a chase starts, CloserShriek1/2 every 3.5-7 s while chasing, CloserGroan1/2 / Snarl / Crack1/2 every 4-9 s when idle and within 80 studs, CloserBreath when you're within 16 studs, joint cracks with crawl steps. All positional at its head, played by each client's copy.

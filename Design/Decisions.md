# Decisions (pre-development, 2026-10-16)

## Core rules
- **Main objective (changed 2026-10-16):** finish the wing's **janitor chore list** to get the key to the next wing. See [[Design/Core Loop]]. Anomaly-spotting and door voting are **dropped** as the main objective (playtest: not fun as the core).
- **Anomalies → Scares:** anomalies are now private scares and loud noises one player experiences while friends go "what are you talking about?". See [[Design/Scares]].
- **Losing:** run ends when everyone is taken. The longer players stay on a level, the more visible the Closer gets and the more often it hunts, so stalling isn't safe.
- **Structure:** 8 wings ("8 Wings Til Dawn"). Each wing is semi-procedural: the hallway is mostly the same, but takes different turns; classrooms are picked from a pool; items are placed in different spots.
  - Layout is randomized when a wing starts: which rooms appear, where chores and items spawn.
- **Solo:** fully playable alone.
- **Revives:** taken players can be revived with Robux or by watching an ad. **Once per run.**
- **Hallucinations:** semi-frequent, more often for low-sanity players.
- **Floor length:** about 8–12 minutes per wing.

## Added 2026-10-16 (playtest round 2)
- **Always first person.**
- **Bigger wings:** main hallway + a side corridor branching off it, bigger classrooms, so players actually split up.
- **Sprint + stamina bar** (Shift). Running out leaves you winded until it recovers.
- **Flashlight with battery** (F). Beam is visible to other players. Batteries are found in classrooms.
- **Blackouts:** every ~2 minutes the lights die; sanity drains (faster with flashlight off) until someone resets the **breaker** in the custodian room.
- **Sanity is visible only on the Staff Wellness Monitor** in the custodian room (per player). Still hidden everywhere else.
- **Custodian room** is a full room: shelves, janitor lockers, cart, mop sink, trash chute, breaker, wellness monitor, notes from the missing janitors.

## Added 2026-10-16 (lobby + dev)
- **Lobby** is its own place (the start place): the janitors' break room. Three NIGHT SHIFT doors; stand in a door's zone (max 4) → 12 s countdown → the group is teleported to a private server of the game place. Invite Friends button.
- Shifts start in the **custodian room**.
- **Dev menu** (F4): spawn/move/hunt/freeze the Closer, jumpscare test, god mode, blackouts, force any scare, finish chores, skip/restart wing, set sanity, fill batteries. Studio, owner, or allowlisted ids only.

## Scope and production
- **First test build:** one wing with chores, the key and exit, a few private scares, and the Closer model (placeholder that Ethan will replace).
- **Target:** playable version by **end of October 2026**, about 16 hrs/week.
- **Art/design:** use **Claude Design** for all visual design (concept art, reference sheets, UI, thumbnail, decals), not Blender (changed 2026-10-16). 3D meshes come from the Creator Store, matched to the Design sheets. See [[Systems/Assets]].
- **Audio:** free assets or self-made.
- **Tools:** Rojo + VS Code + Git (Rojo already connected to Studio).

## Player experience
- **First 2 minutes:** show the key parts of the loop; voice lines from the **Principal (the boss)** say what to do; a note players can re-read.
- **Lobby:** a janitor's break room. Needs to look good. Public servers plus friend invites.
- **Chat:** proximity voice and text. PA/voice mimicry is a **later** feature.
- **Accessibility:** optional; add only what's standard (it's advertised as horror).

## Story and tone
- **Backstory:** the old janitors kept "quitting" without notice. The Principal is grateful you took the gig on short notice and doesn't know what's really happening.
- **Lore:** short easter egg voice lines that rotate so replays don't repeat, e.g. "Kids have been talking about a scary ole' janitor that comes around at night, but that's just kids."
- **Tone:** pure dread. Only the Principal at the start is lighter (oblivious).
- **Target age:** 10 to 13+.

## Progression and money
- **Cosmetics:** mop skins, outfits, earned with coins through a random case system; plus a **journal of achievements**.
- **Coins:** earned only (cannot be bought). A Robux **coin boost** raises how fast you earn. Show case odds in-game anyway.
- **Modes:** Night Shift (normal) and Graveyard Shift (must be unlocked).
- **Purchases:** revives, longer flashlight batteries, sanity pills, funny death effects, custom flashlight beams.
- **Game pass:** a bundle of some of these.

## Launch
- **Name:** **8 Wings Til Dawn.** Thumbnail features the Closer with the title, scary.
- **Promotion:** sponsored ads plus self-made clips.
- **Rollout:** small tests, then a broader launch.

## Technical
- **Server-side:** vote counting and sanity tracking.
- **Saves:** persist which wing the player is on, with **3 save slots**.
- **Multiplayer saves:** the **host's slot** drives the run; everyone earns coins and achievements to their own profile.

- **After death (2026-10-21, design only):** taken players spectate through the CCTV cameras, and every so often get a charge to scare their friends near the camera they watch. Written up in [[Design/After Death]]; nothing built yet. Confirmed 2026-10-21: taken = punishment, scares purely cosmetic (incl. a fake Closer jumpscare), no free rescue (revives purchase-only), room cameras too but the CCTV chore stays halls-only, no Closer scaling. Replaces the "ghost only one player sees" idea.

# After Death: CCTV spectate + scaring friends

Status: **DESIGN ONLY, NOT IMPLEMENTED** (decided 2026-10-21). Do not build until Ethan says go.
Replaces the old line "taken players return as ghosts only one living player can see" ([[Design/Core Loop]], [[Design/The Closer]]).

## Decisions (Ethan, 2026-10-21)
1. A taken player **spectates through the school's CCTV cameras**.
2. **Every so often** the taken player gets the option to **scare their friends**.
3. Being taken is a **punishment**; the scare option is the only thing they get to do. They are not part of the fight.
4. Scares are **purely cosmetic**. They must not affect gameplay: no noise that attracts the Closer, no kill, no sanity change, no chore or door side effects. Examples Ethan gave: slamming a door, the Closer spawning in front of a player and jumpscaring them (a fake one, see below).
5. **No in-game rescue or free revive.** Revives are **only purchasable** (Robux; see [[Design/Decisions]]).
6. Spectator cameras **include rooms** (not only halls). This must **not change the CCTV chore**, which stays **halls only**.
7. The Closer does **not** get stronger when players are taken.

## Why this fits
- The CCTV chore (Monitor, [[Systems/Chores]]) and the security cameras (`SecurityCamera` asset, [[Systems/Assets]]) already exist, so a taken player is now the one watching the monitors.
- It keeps the game's core idea, private scares and doubt between friends ([[Design/Scares]]), but the scares now come from a teammate.
- Being taken stays a punishment (you leave the fight) but you are never just waiting.

## Spectating
- After the kill, the jumpscare plays as now. Then the victim lands on the **camera wall**: hall cameras and room cameras as a grid, click or tab to go full screen. Reuse the Monitor look (scanlines, timestamp, REC dot, "SIGNAL LOST" on static).
- Feeds are the **real world**, not the fake Monitor-chore events: the spectator's camera is moved to a camera CFrame (scriptable camera + CCTV overlay), so they see live players, the Closer, doors and lights.
- **Room cameras are a separate set** from the hall cameras. The CCTV chore keeps using only the hall cameras (tag or attribute on each camera, e.g. `Scope = "Hall"` / `"Room"`, and the chore filters on Hall). Spectate uses both. Rooms that get a camera: classrooms, labs, library, custodian room (decide per wing when building).
- Technical caveat: if StreamingEnabled is on, parts near a camera may not be loaded on the spectator's client. Set `Player.ReplicationFocus` to the camera being watched (or turn streaming off for spectators).
- The Closer is visible on camera to spectators. No pings or markers.
- Cameras can fail: during a **blackout** feeds go to static one area at a time. The Closer standing in front of a lens makes that feed shake and cut out (matches the CCTV chore leap).
- Optional: a camera that is being watched gets a small red light, so living players who look up feel watched.
- If everyone is taken or the wing restarts, spectate ends with the normal restart flow.

## Scaring friends
- **Scare meter:** fills slowly while spectating (about one charge per 60-90 s, one stored at most, maybe two; tune later). When charged, a "SCARE" button lights up on the feed.
- The scare happens **at the camera you are watching**, on living players within about 25 studs of it. Nobody else is affected.
- **Cosmetic only.** Every scare is delivered like the existing private scares: client-side visuals and sound for the viewers in range, nothing the server counts as gameplay. The Closer AI never hears or reacts to a scare.
- Menu (pick one, uses the charge):
  - **Flicker:** the lights stutter for 2-3 s.
  - **Knock / bang:** a loud 3D thump or locker rattle.
  - **Door slam:** a nearby door slams (visual and sound only; it must not lock, block or trap anyone, and must not change a door the player is using).
  - **Radio / PA:** a radio clicks on, or a creepy PA whisper plays ([[Systems/Voice]]).
  - **Fake Closer:** a fake Closer appears in front of the viewer and does a short mini jumpscare (reuse the Closer rig and the CCTV leap style). It is a **client-side illusion**: it cannot hurt, grab or kill, does not trigger the real jumpscare/take logic, music or the Closer's state, and disappears after the scare.
- **Limits (suggested, not yet confirmed):**
  - Does not work on hiding players.
  - Not twice on the same player within about 30 s.
  - A short grace period after a player is taken or a wing starts.
  - Sanity is not changed by scares (cosmetic only).
- No score or leaderboard for scares in v1.

## Revives
- Purchase only (Robux). No free in-game rescue, no Detention room.
- Ethan's earlier note was "Robux or watch an ad, once per run" ([[Design/Decisions]]). This turn he said "only purchasable". Confirm with him whether an ad counts as a purchase or is dropped.

## Build plan (when approved)
1. `Taken` state per player (server attribute), set when the Closer or Schooler takes them; cleared at the next wing / revive.
2. Add a `Scope` attribute to cameras (Hall / Room), place room cameras, make sure the CCTV chore filters on Hall.
3. Client `SpectateController`: camera wall UI in the Monitor/UiTheme styling, scriptable camera onto camera CFrames, ReplicationFocus handling.
4. Server `SpectateScareService`: scare charge timer, validates range / cooldowns, fires the private-scare delivery to eligible viewers (no gameplay effects).
5. Fake Closer scare: client-only rig, short lunge, flash, sound; make sure it cannot trigger the real take logic.
6. Hook blackouts and Closer-near-lens effects into the feeds.
7. Dev menu: "Take me" -> spectate, "Charge scare".
8. Playtest with 2+ players, then the purchasable revive.

Related: [[Design/The Closer]] (kill and taken state), [[Design/Scares]], [[Systems/Chores]] (Monitor), [[Systems/Voice]].

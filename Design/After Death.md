# After Death: CCTV spectate + scaring friends

Status: **DESIGN ONLY, NOT IMPLEMENTED** (brainstormed 2026-10-21). Do not build until Ethan says go.
Replaces the old line "taken players return as ghosts only one living player can see" ([[Design/Core Loop]], [[Design/The Closer]]). Revives (Robux / ad, once per run) are untouched and still planned.

## What Ethan decided
1. A taken player **spectates through the school's CCTV cameras**.
2. **Every so often** the taken player gets the option to **scare their friends**.

Everything below that is not those two points is a suggestion to react to, not a decision.

## Why this fits
- The CCTV chore (Monitor, [[Systems/Chores]]) and the security cameras (`SecurityCamera` asset, one at the start of each hall, [[Systems/Assets]]) already exist, so dead players get a job in the same fiction: you are now the person watching the monitors.
- It keeps the game's core idea, private scares and doubt between friends ([[Design/Scares]]), but the scares now come from a teammate, which is funnier and worse.
- Being taken stays a punishment (you leave the fight), but you are never just waiting.

## Spectating (suggested)
- After the kill, the victim's jumpscare plays as now. Then instead of a plain black screen they land on the **camera wall**: the hall cameras (plus custodian room / classroom cameras if we add them) as a grid, click or tab to go full screen. Reuse the Monitor look (scanlines, timestamp, REC dot, "SIGNAL LOST" on static).
- Feeds are the **real world**, not the fake Monitor-chore events: the spectator's camera is moved to the `SecurityCamera` CFrame (scriptable camera + CCTV overlay), so they see live players, the Closer, doors and lights.
- Technical caveat: if StreamingEnabled is on, parts near a camera may not be loaded on the spectator's client. Set `Player.ReplicationFocus` to the camera being watched (or turn streaming off for spectators).
- The Closer is **visible on camera** to spectators. They can talk about it over voice chat; no pings or markers (spectating stays a flavourful extra, not a second Closer-radar).
- Cameras can fail: during a **blackout** feeds go to static one hall at a time. The Closer standing in front of a lens makes that feed shake and cut out (matches the CCTV chore leap).
- Optional: a camera that is being watched gets a small red light, so living players who look up feel watched. Cosmetic, cheap, on theme.
- If everyone is taken or the wing restarts, spectate ends with the normal restart flow.

## Scaring friends (suggested)
- **Scare meter:** fills slowly while spectating (about one charge per 60-90 s, one stored at most, maybe two). When charged, a "SCARE" button lights up on the feed.
- The scare happens **at the camera you are watching**, on living players within about 25 studs of it. Nobody else is affected, so it is local and fair.
- Menu of small scares (pick one, each uses the charge):
  - **Flicker:** the hall lights stutter for 2-3 s.
  - **Knock / bang:** a loud 3D thump or locker rattle at the camera.
  - **Door slam:** a nearby unlocked classroom door slams (never one a player is using, never a locked one, so it cannot trap anyone).
  - **Radio / PA:** a radio clicks on, or a creepy PA whisper plays in that hall (reuses [[Systems/Voice]] lines).
  - **Shadow at the end of the hall:** the existing private-scare figure, shown to viewers in range.
- **Limits so it stays fun and fair:**
  - Never lethal, never moves or traps a player, never blocks a chore.
  - Does not work on hiding players (hiding keeps you safe from scares, which rewards it).
  - Small sanity drain only, and not twice on the same player within about 30 s.
  - A short grace period after a player is taken or a wing starts.
- **Risk and reward (optional knob):** each scare makes noise that can pull the Closer to that hall, so a ghost can accidentally get a friend caught. Easy to tune: noise strength 0 = purely cosmetic.
- Trolling stays friendly: the spectator sees when a friend reacts (sprint burst, flashlight swing). No score or leaderboard for scares in v1.

## Open questions for Ethan
1. Is being taken mostly a punishment, or should the dead feel like part of the fight? (This doc leans on "mostly a punishment with a fun side job".)
2. Should scares make noise that can attract the Closer (risk), or be purely cosmetic?
3. Should there be a **free in-game rescue** (e.g. a Detention room; living players do a short risky task to get a taken player back), with the Robux / ad revive as the skip? Not decided, not in this doc's scope.
4. Do the cameras include rooms, or halls only?
5. Does the Closer scale up when players are taken (faster / more aggressive), so losing someone early matters?

## Build plan (when approved)
1. `Taken` state per player (server attribute), set when the Closer or Schooler takes them; cleared at the next wing / revive.
2. Client `SpectateController`: camera wall UI in the existing Monitor/UiTheme styling, scriptable camera onto `SecurityCamera` CFrames, ReplicationFocus handling.
3. Server `SpectateScareService`: scare charge timer, validates range / cooldowns, fires the existing private-scare delivery to eligible viewers.
4. Hook blackouts and Closer-near-lens effects into the feeds.
5. Dev menu: "Take me" -> spectate, "Charge scare".
6. Playtest with 2+ players.

Related: [[Design/The Closer]] (kill and taken state), [[Design/Scares]], [[Systems/Chores]] (Monitor), [[Systems/Voice]].

# Core Loop

## Per wing (8–12 minutes)
1. **Clock in:** the Principal's clipboard lists this wing's chores (also the re-readable note).
2. **Split up and work:** players spread out to finish chores. More players = more chores, scaled so solo stays doable.
3. **Get the key:** when the list is done, the wing key appears (e.g. in the custodian closet or the Principal's office).
4. **Exit together:** unlock the door to the next wing. Players who aren't at the door when it opens are left behind briefly, and the Closer notices.

## Chore types (mini-games, Among Us style)
You can't move while doing one (you're exposed). W/A/S/D or X backs out; walking away cancels it.
- **Lock a classroom:** key ring. Find the key tagged with the room number, then hold the lock to turn it.
- **Mop a spill:** scrub the stain away by rubbing.
- **Erase a whiteboard:** rub the writing off.
- **Rewire a light panel:** drag 4 colored wires to matching terminals.
- **Set a thermostat:** stop the needle in the green zone 3 times in a row.
- **Fix the leaking pipe** (custodian room): crank the valve wheel clockwise.
- **Empty trash:** carry the bag to the trash chute (no mini-game; your hands are full).
- **Replace dead lights** (halls): carry light tubes from the custodian room.
- **Hold the ladder** (co-op, 2+ players): one holds, one climbs and changes a flickering tube. Let go and they fall (loud).
- **Vacuum or sweep a hall:** vacuum fast + loud, push broom slow + quiet.
- **Restock the flashlight charger** (office): carry battery boxes; unlocks spare batteries for the team.
- **Watch the security cameras** (office): report every movement on the feed; sometimes it's the Closer.
- **Scrub graffiti:** at low sanity it spells your name or a teammate's.
- **Answer the PA phone** (office): 3 right answers in a row (answers are in the building). Breathing on the line: hang up fast.
- **Put the chairs up** in a classroom.
- **Unclog a toilet** with the plunger: timing, gross and loud.
- **Reset the clocks** to shift time (master clock in the office). At low sanity they all read 3:33.
- Tools live on the hotbar (3 slots, see [[Systems/Inventory]]).
- Server checks you stayed by the task and didn't finish impossibly fast.

## Crouching and hiding
- **C** toggles crouch: slower, harder for the Closer to spot (sight range x0.6).
- **Hide under any teacher's desk** (and the custodian's desk): crouch-hide prompt, Space to crawl out.
- **Truly hidden** only if, at the moment you hid, the Closer hadn't seen you for **2.5 s** and was **22+ studs** away. Otherwise it saw you go in, walks to the desk and drags you out.
- If it loses you, it searches your last spot for a few seconds, then moves on.

## Pressure
- The longer a wing takes, the more visible the Closer becomes and the more often it hunts (no stalling).
- The Closer **undoes work**: unlocks a locked door or leaves a new spill, at most 2 times per wing and never after the list is finished.
- Working alone is faster but riskier: the Closer targets isolated players.

## Power and light
- Flashlight battery drains while on (~90 s). Batteries found in classrooms go in your pocket (max 3); press **R** to swap one in. Fewer pickups per wing.
- In darkness your eyes slowly adjust: a faint glow so you're never fully blind.
- Random blackouts: lights out, sanity drains, reset the breaker in the custodian room.

## Hidden sanity
- Drops: near the Closer, alone in the dark, experiencing scares, flashlight dead.
- Low sanity → more private scares. (Fake chores were removed 2026-10-21: nothing fake is ever added to the chore list.)
- Shown only on the Staff Wellness Monitor in the custodian room. Elsewhere, hints: color loss, heavier breathing, footsteps out of sync.

## The Closer
- See [[Design/The Closer]]. Takes players; taken players spectate through the CCTV cameras and can scare their friends (planned, see [[Design/After Death]]; replaces the old ghost idea).
- Run ends when everyone is taken.

## Coins and progress
- Earn coins per chore, per wing cleared, and for finding missing-janitor belongings (optional collectibles).
- Save slot stores the wing reached (host's slot in multiplayer).

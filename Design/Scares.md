# Scares

Former "anomalies". No longer the objective: they are **private scares** that hit one player (sometimes the whole group), so friends end up asking "what are you talking about?"

## Rules
- Most scares are **private**: only one player sees or hears them. Others see nothing.
- Some are **shared** (everyone hears a locker slam) to keep the group on edge.
- Low sanity → more private scares. **No fake chores** (removed 2026-10-21, Ethan's call).
- Some scares cost sanity; none directly kill (only the Closer kills).
- Each is a module with `apply()` / cleanup (system already built; reuse it).

## Loud / sudden
- Locker slams shut right next to you (P)
- Fire alarm shrieks for 2 seconds, then silence (P/S)
- Bell rings at 2 a.m. (S)
- Something heavy drops in the ceiling above you (P)
- A classroom door bangs open as you pass (P)
- Basketball bouncing in the empty gym, getting closer (P)

## Seen only by you
- A student standing perfectly still at the end of the hall; gone when you turn back (P)
- **Your own avatar** sitting in a classroom (P)
- A locker hanging open with your username inside (P)
- Class photo with an extra student; next time, it's you (P)
- Chalkboard message addressed to you by name (P)
- A teammate's avatar walking the wrong way (P)
- Shadow behind a frosted door window (P)

## Audio whispers
- PA announcement that reads out your username (P)
- Kids laughing from an empty classroom (P)
- Footsteps matching yours, one beat late (P)
- A chat bubble over a teammate saying something they never typed (P, later)

## Fake chores (hallucinations) — REMOVED 2026-10-21
Ethan: no fake tasks on the chore list. The `FakeSpill` scare is out of ScareDirector's pool (module kept in `shared/Scares/FakeSpill.luau`). Notes below are history.
- A spill only you see, in a room far from the group
- A classroom on your lock list that doesn't exist
- A trash bag that's heavier than it should be; it moves

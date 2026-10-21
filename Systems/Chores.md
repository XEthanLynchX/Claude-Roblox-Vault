# Chores

Server: `src/server/Services/ChoreService.luau` (prompts, list, completion). Task screens: `src/client/Controllers/Minigames.luau`. Tuning: `Config.ChoreTuning` and `Config.Minigames.MinSeconds` (anti-cheat).

## The list (per wing)
Newer and classic chores alternate (2 newer, 1 classic). Newer: Phone, Bulbs, Chairs, Vacuum, Graffiti, Plunger, Clocks, Monitor, Restock. Classic: Trash x2, Spill x2, Board, Rewire, Thermostat, Sink.
"Hold the ladder" (ladderChore) is out of the mix since 2026-10-20 (Ethan asked); the code is kept.

## How each one plays (2026-10-20)
- **Answer the PA phone**: 3 questions in a row. The first two are ordinary; the last is creepy only `PhoneQuestions.CreepyChance` (25%) of the time. "What's the name of this school?" -> Dawnridge High (plaque by the Principal's notice on the main start wall).
- **Replace the dead lights**: tubes from the box in the custodian room. Prompt sits on the fixture and only shows while you look within 14 degrees of it (prompt attribute `LookAt`, handled by PromptUi).
- **Put the chairs up**: unchanged.
- **Vacuum or sweep**: hold the dirt prompt with the vacuum (fast, loud) or broom (slow, quiet). Server sets player attribute `ToolAction` while working -> arm animation for everyone else (ToolAnim).
- **Scrub graffiti**: the tag is placed on the real wall surface (raycast against Shell), with paint drips; scrub task.
- **Unclog the toilet**: needs the plunger. Side skill check (needle + red band, SPACE/click), 3-5 hits (`PlungesNeeded = {3, 5}`). Every press is a plunge: noise, sound, `ToolStroke` attribute -> stroke animation.
- **Reset the clocks**: label names the rooms still wrong ("Reset the wrong clocks in Room 104, Room 112 ... (1/3)").
- **Watch the cameras**: feed sits 0.7 studs in front of the lens. Uses Assets.SecurityCamera (pivot at the lens, -Z).
- **Restock the charger**, **Take out the trash**: unchanged.
- **Mop a spill**: carry the mop (Tools.Mop in the custodian room, placed when a spill exists). Hold left click within `MopRange` (6.5) of the puddle: remote `Mop` start/stop, server adds time while you're in reach, puddle scales down, done at `MopSeconds` (7). Several people mop faster. MopController shows "HOLD [LMB] TO MOP" + a drain bar; touch gets a MOP button. Closer-made spills (AddSpill) work the same.
- **Erase the whiteboard**: DONE only when every cell is clean (cells under 0.15 count as clean and are drawn clean).
- **Rewire**: the room's plain panel is hidden and the old breaker box (Assets.BreakerBox) placed on it. Task screen: drag sagging cables from the conduit to the brass terminal with the same colour band; each breaker clicks on.
- **Set the thermostat**: no skill check. Starts at 50-56 degrees, + / - buttons, it drifts down 1 degree every 1.1 s if you stop; land on exactly 68 and let it settle 0.6 s.
- **Fix the leaking pipe**: same drag-the-wheel idea, redrawn (rusted pipe spraying water, red handwheel, PSI gauge, LEAKING -> SEALED).

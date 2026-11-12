# Redesign Plan (November 2026)

Written 2026-11-10 from Ethan's feedback and a review of the code in Studio. Nothing in it is built yet.
**To use it, say "work on Phase N".** Each phase stands on its own: goal, what to build, which scripts it touches, and how to tell it's done.

Related: [[Design/Decisions]] · [[Design/Core Loop]] · [[Design/After Death]] · [[Design/Monsters]] · [[Systems/Chores]] · [[Systems/Map]]

---

## How Claude works a phase
1. Read this note, [[Handoff]] and the notes the phase links to. Check the "Decisions" section for anything Ethan changed.
2. Do **only that phase**. If something outside it needs changing, write it under "Found while building" at the bottom instead.
3. Test as the phase says (solo, and with 2+ players where it says so). A Studio playtest with no new console errors is the minimum.
4. Export every changed script to `src`, check the hashes, and commit ([[Systems/Conventions]] → Rojo sync rule).
5. Tick the phase in the tracker below, add a [[Log]] entry, and update the system notes the phase names.

Sizes: **S** = one session, **M** = 1–2 sessions, **L** = 2–3+ sessions.

## Tracker
| #      | Phase                                                                              | Size | Needs first |
| ------ | ---------------------------------------------------------------------------------- | ---- | ----------- |
| [ ] 1  | Five wings, the name, remove the Schooler                                          | M    | —           |
| [x] 2  | Shared chores (built 2026-11-10; 2-player test + TutChores voice line still to do) | M    | —           |
| [ ] 3  | Eye height: players stay small, the view doesn't                                   | S–M  | —           |
| [ ] 4  | Spectate friends (follow cam + nearby cameras)                                     | M    | —           |
| [ ] 5  | The Security Desk (all cameras, for living players)                                | M    | 4           |
| [ ] 6  | ID badges: carry your friend out                                                   | M    | — (4 helps) |
| [ ] 7  | School Rules system + Rule 1 (Roll Call)                                           | L    | 1           |
| [ ] 8  | Rules 2 and 3 (Lights-Out Drill, Eyes Front)                                       | L    | 7           |
| [ ] 9  | Rules 4 and 5 (Quiet Hours, The New Hire) + stacking                               | L    | 8           |
| [ ] 10 | Wing atmosphere (light, dressing, layout, mess; no themes)                         | M–L  | 1           |
| [ ] 11 | Wing conditions (random modifiers per run)                                         | M–L  | 10          |
| [ ] 12 | Light you can live with (batteries + a dead-flashlight fallback)                   | S–M  | —           |
| [ ] 13 | First-person hands (janitor gloves holding everything)                             | M    | 3           |
| [ ] 14 | Custodian room tools + item fixes (vacuum in the floor, trash bag icon)            | S–M  | —           |
| [ ] 15 | Close calls I: The Interrogation ("where are the others?")                         | L    | 2           |
| [ ] 16 | Close calls II: hold your breath, hold the door, flashlight stare-down             | M–L  | 15          |
| [ ] 17 | The lobby showpiece (a trapped monster of our own)                                 | M    | —           |
| [ ] 18 | Tutorial, text and a full-run test                                                 | M    | all         |

Suggested order (updated 2026-11-10): 2 ✓ → 12 → 14 → 3 → 13 → 1 → 6 → 4 → 5 → 7 → 8 → 9 → 15 → 16 → 10 → 11 → 17 → 18. Phase 14 is quick bug fixes, so it goes early. Phases 2, 12, 3 and 6 fix what playtesters feel first (3 and 13 both work on the first-person camera, so do them back to back); 1 has to land before the rules (one rule per wing).

---

## What's wrong now (found 2026-11-10)
1. **Chores are solo work.** Every chore belongs to one player (`Owner` on each entry) and other players' prompts are hidden (PromptUi). A crew of four runs four separate to-do lists, and one slow player holds the wing back.
2. **Every wing looks and plays the same.** All wings come from the same generator with the same room-theme weights (`WingBuilder` THEMES), the same hall dressing and the same chore pool. By wing 3 players have seen everything.
3. **The run is long.** 8 wings × 8–12 min = 64–96 min, plus the shift board (up to 60 s) and the intro (up to 110 s). Top Roblox games average 35–45 min sessions.
4. **The eyes are low.** Players are 0.8 scale (`Config.Characters.Scale`) so they fit in lockers and under desks, and the first-person camera sits at that small body's head.
5. **Death is boring and has no stakes.** A taken player gets about 35–40 camera feeds (one per room plus one per hall run, `SpectateCams`) and can't find their friends. Every taken player also comes back free at the next wing (`WingService.buildWing` respawns everyone), so being taken barely matters.
6. **The scares ask nothing of the player.** They're private and harmless, so players learn to ignore them. Doors-style threats each have one cue and one correct response.

---

## Decisions (2026-11-10)
Ethan's calls, plus Claude's recommendations marked **(default)**. A default stands unless Ethan changes it before that phase starts.

- **Wings: 8 → 5** (Ethan, 2026-11-10). 5 wings × ~8 min plus board, intro and dawn ≈ 45 min, the length of a top Roblox session. Five is also a buildable number of School Rules. Six is the fallback if 5 feels short (≈ 55 min, plus one spare rule from the list below).
- **Name: "5 Wings Til Dawn"** (Ethan, 2026-11-10). Renamed in Phase 1. Thumbnails are Ethan's pick at launch.
- **All chores are shared.** Anyone can do any chore, and losing a player shrinks the list instead of burying the others.
- **School Rules: one per wing.** 5 rules for 5 wings. Each wing's own rule is its headline; earlier rules keep showing up, less often (they stack).
- **Height: lift the camera, don't rescale.** Bodies stay 0.8 so hiding keeps working; the first-person eye goes up to normal adult height. Scaling both players and the Closer would mean rebuilding doors (8 tall, Closer 7.5), lockers, desk hides, ladder heights and catch heights.
- **ID badges** (Ethan, 2026-11-10). A taken player drops their ID badge where they fell. **Nobody can pick up the wing key while a badge is lying on the floor**: the crew has to collect every badge first, then the key, then the exit. Carried badges bring their owners back next wing. The R$25 revive stays as the fast way back. **Lost & Found** (Ethan asked for a fairer rule, 2026-11-10): if someone is taken *after* the key is picked up and their badge is left behind when the crew exits, the badge isn't lost. It turns up in the next wing's **Lost & Found** bin, and like any badge it blocks that wing's key. The moment a teammate picks it up, its owner wakes up in the custodian room with a flashlight. Nobody is ever out for a whole run because of bad timing: the worst case is sitting out the start of one wing, and the crew pays with a detour.
- **Spectating.** Taken players follow their friends by default (Q/E to switch). The camera view shows only the cameras near living friends. The full camera network moves into the living game as the **Security Desk** in the main office.
- **The New Hire doesn't kill** (Ethan, 2026-11-10): its lunge is a jumpscare, sanity −30, and the Closer is called in to hunt you right away. The Closer stays the only thing that takes players.
- **Hands in first person** (Ethan, 2026-11-10): you see a pair of hands holding the flashlight, the journal and every hotbar item. Default: **janitor viewmodel arms** (navy work-shirt sleeves and worn work gloves) instead of your avatar's real arms. Avatars vary too much (packages, accessories, the 0.8 scale) and real arms clip the camera. Other players still see your real avatar holding things, as now. If the real arms look fine in a test, they can be used instead.
- **Batteries stay a coin/Robux item, but running out can't mean blindness** (Ethan, 2026-11-10). Every player starts each wing with batteries (Ethan, 2026-11-12). The charger chore gives one spare battery per player present. Batteries are easier to find, and a dead flashlight can still be shaken for a few seconds of weak, noisy light. See Phase 12.
- **More close calls** (Ethan, 2026-11-10): more moments where the Closer *almost* gets you and you can still survive with a skill check or a hard choice, like the locker skill check. The first is Ethan's **Interrogation**: it grabs you and demands you give up a friend. See the Close calls section and Phases 15–16.
- **The Schooler is removed** (Ethan, 2026-11-10: "it's broken and doesn't work"). The Closer is the only monster until each School Rule gets its own (below). Done in Phase 1.
- **Monsters = rules** (Ethan, 2026-11-10): the old 8-monster plan is replaced. Only the five monsters tied to the School Rules stay in [[Design/Monsters]] (Principal, Overdue, Hall Monitor, Lunch Lady, Specimen); each will later replace its rule's stand-in. The rest were removed.
- **No wing themes** (Ethan, 2026-11-10): no per-wing room mix or wing names; that would mean new room types and moving tasks. Wing variety comes from light, hall dressing, layout size, mess and the random conditions/events (Phases 10–11).
- **A lobby showpiece** (Ethan, 2026-11-10): the lobby needs something that builds dread before a shift, like the trapped monster in another game, but our own. Phase 17.
- **This replaces** these parts of [[Design/After Death]] (2026-10-21): "no free in-game rescue" (the badge is a rescue now) and "taken players spectate through the CCTV cameras" (cameras are now the second view, not the main one). It also replaces the "8 wings" lines in [[Design/Decisions]], [[Design/Setting]] and [[Design/Monsters]].

---

## The School Rules
Each rule has one **cue** you hear before it acts, one **response** that keeps you safe, and one **consequence**. Rules are never explained up front: players learn them by surviving or by being taken, exactly the way Doors teaches its entities (see Teaching below). Every rule is built from systems and assets the game already has. Each one can later get its own monster model from [[Design/Monsters]], which turns the rules into that plan's cheap first version.

| Wing | Rule | Cue | Do this | Break it and | Built from | Later monster |
|---|---|---|---|---|---|---|
| 1 | **Roll Call** | PA chime, then a breaking voice counts down from 10 | Be hidden (locker or under a desk) by zero | The Closer comes in (even if it was outside) and hunts the nearest exposed player. Nobody exposed: menace drops 10 | TheCount scare's counting voice, HideService, CloserService | The Principal |
| 2 | **Lights-Out Drill** | Drill tone; hall lights turn red | Stand still with your flashlight off until the all-clear (6–8 s) | A big noise at your spot; if the Closer is in, it hunts you, otherwise menace jumps | RedLight scare, LightFx.push "red", NoiseService | The Overdue |
| 3 | **Eyes Front** | Pencil scratching; a still figure at a junction | Keep it on someone's screen. It only moves when nobody living is looking | It reaches you, "writes you up": frozen 3 s and a whistle that tells the Closer where you are | StillStudent figure (faceless fallback), line-of-sight checks | The Hall Monitor |
| 4 | **Quiet Hours** | "Quiet hours" chime and a ticking clock (30–45 s) | No loud work: no vacuum, plunger, sprinting, chairs or chute. The broom is fine | Loud actions are heard wing-wide and start an instant hunt | NoiseService multiplier, existing noise sources | The Lunch Lady |
| 5 | **The New Hire** (finale) | PA: "Please welcome our new hire." Dripping and glass clinks nearby | Keep away from the janitor with no flashlight beam and a flickering name tag | Get within ~6 studs and it lunges: jumpscare, sanity −30, and the Closer is called in to hunt you right away | Avatar-copy fallback in the OwnAvatar scare, ScareUtil | The Specimen |

- **Stacking:** in wing N, rule N fires most (every 60–120 s after a ~90 s grace). Rules 1 to N−1 fire about a third as often. Wing 5 has all five.
- **Never during:** a hunt, the intro, the tutorial, a revive grace period or dawn. At most one rule event at a time.
- **Teaching, the Doors way** (Ethan, 2026-11-10: "do what Doors does"). No tutorial text or PA explanation; the cue is the only warning. When a rule gets you taken, the death screen shows a hint about that rule before spectating starts, on **every** death, not just the first. The hints get more precise the more often the same rule has taken you (Doors' Guiding Light works this way). The count is saved per account. Example for Lights-Out Drill: 1st "The halls turned red… something was listening for movement." → 2nd "When the drill tone sounds, stop where you are." → 3rd+ "Drill tone = stand still, flashlight off, until the second tone."
- **Rule Book:** a journal page per rule. It shows the rule's name once you've met it, and the plain-words hint once it has taken you, written as a scrawled note from the janitor before you.
- **"You learned something" cue:** the death-screen hint ends with *"Written in your journal."* Next time you're in the school (respawn, badge return or next wing), a page-turn sound plays and a small card slides in: **JOURNAL UPDATED · Lights-Out Drill**. The journal key hint pulses until you open it, and the new page has a red tab and a "NEW" stamp until you've read it. Meeting a rule for the first time adds its name the same way.
- **Spare rules (if the game goes to 6 wings):** *Occupied*: never open a shut stall (StallFeet), even when the plunger chore is in there.
- The existing private scares stay as flavour between rule events. TheCount and RedLight keep their solo versions.

---

## Close calls
A close call is a moment where the Closer *almost* gets you and you can still get out. Each one has a clear tell, a short window and a cost even when you win, so surviving feels earned. The locker skill check is the model. Shared rules:
- **Limited:** each close call can save a player at most once per wing. After that, the same situation takes you.
- **Always a cost:** sanity, noise, a used item, or (in the Interrogation) a friend.
- **Readable:** a distinct sound and screen treatment, so clips make sense to viewers ("the grab", "the breath meter").
- **Never in solo** if it needs another player (the Interrogation); solo players get the other close calls.
- The Closer stays the only thing that takes players, so badges and death hints work the same.

### The Interrogation (Ethan's idea): Phase 15
- **When:** the Closer catches a player who still has living teammates, at most once per player per wing, and not if they're the last one alive.
- **What happens:** instead of the kill, it lifts you off the ground facing it. The screen goes dark at the edges and its **creepy voice** asks: *"Where are the others?"* A **6 s timer** runs down. Your living teammates' names appear as choices, plus **"I won't tell"** so you don't have to sit through the timer.
- **Give up a friend:** it drops you (sanity −25, stunned ~2 s, then free) and says *"Good."* It is **not** instant: it walks away from you, jumps into the nearest vent, jumps out of the **vent nearest that friend**, and chases them down. The friend's only way out is to be **in a locker or under a desk** when it gets there; it still searches, so they have to pass the locker skill check (or the desk breath check from Phase 16). Anywhere else, it finds them.
- **"I won't tell", or the timer runs out:** *"Liar."* You're taken, as a normal catch.
- **Voice lines (new, creepy voice pack):** "Where are the others?", "Who else is here?", "Tell me… and I'll let you go.", "Good.", "Liar.", "Hide and seek, then." Made with `tools/make_voices.py`; Ethan uploads.
- **Why it works:** a hard choice instead of an instant death, betrayal between friends (the game's "doubt your friends" theme), and very clippable.

### More close calls: Phase 16
- **Hold your breath** (desk hiding): when the Closer stops right next to your desk, hold a key to hold your breath. A breath meter drains; let go too early and it hears you; run out of breath and you gasp and it finds you. The desk's version of the locker skill check.
- **Hold the door** (Ethan's rules): if a player shuts a door during a chase, the Closer sprints to it (faster than its normal chase speed) and starts pounding. There's no "hold" prompt: the player is put straight into a **5 s mash** to keep it shut. Mash fast enough and it gives up and loses you; too slow and the door bursts open and it takes you. The mash gets harder each wing.
- **Flashlight stare-down:** once per hunt, shine your light straight in its face at the moment it lunges (a timing check). It recoils for 2 s and the chase goes on. It costs a big chunk of battery and fails with a low battery.
- **Last-second dodge:** at the end of a chase, a left/right dodge prompt as it lunges; a miss gives you 2 s.

---

## Phase 1: Five wings (and the name)
**Goal:** a run is 5 wings and ends at dawn after wing 5.
**Touches:** `src/shared/Config.luau`, `src/shared/TutorialLines.luau`, `src/shared/Lore.luau`, `src/client/Controllers/ShiftScreens.luau`, `src/server/Services/HostSave.luau`. If renamed: `LobbyClient`, `Lobby/BreakRoom`, `CrewService`, `AfkService`.

Build:
1. `Config.WingCount = 5`.
2. `Config.Closer.Wings` has 8 per-wing entries. Cut it to 5 by keeping old wings 1, 3, 5, 7 and 8, so the first and last wings feel the same as now. The `{ Wing 1, Wing 8 }` pairs elsewhere already blend by `WingCount`; just change their comments to `{ first wing, last wing }`.
3. **Remove the Schooler** (Ethan: it's broken). Delete `SchoolerService`, `SchoolerFx` and `Config.Schooler`, and its model in ServerStorage (`TheSchooler`, moved to `ServerStorage.AssetBackups` first, then deleted once a playtest is clean). Remove every reference: WingService (Start / NewWing / Dawn / onTaken), the dev menu buttons (`schooler_*`), Sounds (`Schooler*`), Movement / MusicController / ScareDirector / LibraryService if they mention it. The Blackthorn Library level was its level and stays off. Grep `Schooler` until nothing is left.
4. Old host saves: `HostSave` already clamps a saved wing to `WingCount`, so a save on wing 6–8 loads wing 5. Test it.
5. Text and voice: `TutorialLines.TutEight` ("There are eight wings…") needs a new line and new audio (`tools/make_voices.py`; Ethan uploads it, then the id goes in `Config.Sounds`). Change the Lore line "Counted the wings on the map. Eight. I walked nine." to "Five. I walked six." Check the shift board's wing strip lays out well with 5 boxes. The journal's "Wing n of N" and DawnService already follow `WingCount`.
6. **Rename to "5 Wings Til Dawn":** lobby title (`LobbyClient`, `BreakRoom` sign), kick messages (`CrewService`, `AfkService`), the lobby graphics note, the game's name and description on the Creator Dashboard, and this vault's titles. (Thumbnails wait for launch; Ethan picks them.)

Done when:
- [ ] A solo run reaches dawn after wing 5.
- [ ] A host save made on wing 7 loads on wing 5.
- [ ] No "eight" left in text or voice (grep `[Ee]ight`, `8 Wings`).

## Phase 2: Shared chores
**Goal:** anyone can do any chore. The crew finishes the list together and faster, and one slow or AFK player can't hold the wing back.
**Touches:** `src/server/Services/ChoreService.luau`, `src/client/Controllers/PromptUi.luau`, `HudController.luau`, `JournalBook.luau`, `TutorialLines` (TutChores), [[Systems/Chores]].

Build:
1. `ChoreService.Setup`: stop giving chores owners (`assignTo = nil` for every chore). `mine()` then always allows the player.
2. Tools: `placeTool` puts one copy per owner, so with no owners only one mop would appear. Place one copy per crew member instead (pass the crew size in), so nobody waits on a mop.
3. **"On it" tag:** when someone starts a chore (minigame start, takes the trash bag, starts mopping or vacuuming, takes a tube), set `ActiveBy = DisplayName` on the entry, and clear it on cancel or finish. The HUD list and journal show "Sam is on it" so two people don't walk to the same chore. Multi-part chores (bulbs, chairs, vacuum patches, mopping) already go faster with two people.
4. **Losing someone** (taken, left, AFK): shrink the list instead of handing their chores out. Strike `floor(remaining / aliveBefore × 0.5)` unstarted chores (0.75 when only one player is left). This keeps the "nobody gets buried" idea from `Reassign`. Rewrite `RemovePlayer` and `Reassign` around it; `Reclaim` goes (a revived player just joins in).
5. Completion credit: `complete()` gets the player who finished the chore. Their TitleService "Chores" stat counts it; coins still go to everyone (`AwardAll`).
6. Client: `PromptUi` stops hiding prompts by `ChoreOwner`; the HUD and journal drop owner names. The tutorial card and the **TutChores voice line** ("Each of you gets your own chore list…") change to something like "The chore list is shared. Split it up." This needs new audio.
7. Tune `Config.Chores` (Base 5, +2 per extra player, Max 12) after a 2-player test. Aim for 6–9 min per wing.

Done when:
- [ ] In a 2-player test, both players see and can use every chore prompt, and there are enough tools for both.
- [ ] A player leaving shrinks the list, with a clear message.
- [ ] "That's on X's list" never appears.
- [ ] [[Systems/Chores]] and [[Design/Core Loop]] updated.

## Phase 3: Eye height (players stay small, the view doesn't)
**Goal:** the first-person view sits at normal adult height. Bodies stay 0.8 so lockers, desks and the ladder keep working, and the Closer still towers.
**Touches:** `src/shared/Config.luau`, `src/client/Controllers/Movement.luau`, `FirstPersonBody.luau`, `ToolAnim.luau`, maybe `CloserFx.luau` / `CloserGaze.luau`.

Build:
1. Add `Config.Characters.EyeLift = 0.8` (studs; tune between 0.6 and 1.0). In `Movement`, the standing camera target becomes `EyeLift` instead of `0`. Crouch (−1.6), hidden (−1.9) and desk (`DeskCameraY`) keep their values, so hiding views stay low.
2. `FirstPersonBody`: with the higher camera, make sure the neck and shoulders don't show and held tools are still in frame. Held-item offsets may need the same lift.
3. Make the Closer *feel* bigger without changing its collision (pick one or both):
   - **Loom:** when it's within ~14 studs and on screen, add a little `FovPull` (CloserGaze already does this when you stare) and drift the camera pitch up.
   - **Taller look:** visual height 7.5 → ~8.3 while `AgentHeight` stays 7.5, with a short duck when it passes under a door frame (8 tall).
4. Recheck everything that depends on where the eye is: dead-light prompts (`LookAt`), whiteboard, graffiti and clock prompts, the door peek, the locker peek, desk-hide entry, the jumpscare framing and the dawn line-up shot.

Done when:
- [ ] Ethan and a friend agree the standing view feels like a normal height.
- [ ] Hiding, the ladder and the jumpscare look the same as before.

## Phase 4: Spectate friends (follow cam + nearby cameras)
**Goal:** a taken player lands on their friends, not on 40 cameras.
**Touches:** `src/client/Controllers/SpectateController.luau`, a new small server handler for the watched target (in `AfterDeathService` or a new `SpectateService`), `Remotes`.

Build:
1. After the jumpscare, open in **FOLLOW** mode: a third-person camera over a living teammate's shoulder (`CameraSubject` = their Humanoid, so the normal camera pops in front of walls). Q/E (LB/RB) cycles living teammates. On screen: their name, what they hold ("has the key"), and where they are ("west hall").
2. Streaming is on, so the client tells the server who it watches (validated: a living teammate, rate-limited), and the server sets `player.ReplicationFocus` to that teammate's root.
3. **CAMERAS** tab (Tab key / a button): the existing CCTV feed, filtered to the 1–2 cameras nearest each living teammate (the room camera if they're inside a room). That's 2–6 feeds labelled "near Sam" instead of 40. Keep the scanlines, REC and static look.
4. **Camera in awkward spots** (Ethan: no visual issues for the spectator):
   - **In a locker:** show the locker from the outside at a fixed angle (the body is hidden, HidePose Locker), with a "hiding" tag.
   - **Under a desk:** a fixed low angle beside the desk, never inside the desk mesh.
   - **Against a wall or in a tight corner:** the camera pulls in and up instead of going through the wall (a raycast from the head to the camera spot, kept off walls by ~0.5 studs). If there's no room, switch to over-the-head.
   - Never put the camera inside props, ceilings or doors. Fade through any part that briefly blocks the view rather than snapping.
5. Keep the after-death shop button where it is.
6. Space for the badge status line from Phase 6.

Done when:
- [ ] Taken in a 3-player test, you can find and watch each friend within two key presses.
- [ ] No streaming holes (missing walls or props) on the follow cam or the filtered feeds.

## Phase 5: The Security Desk (all cameras, for living players)
**Goal:** the full camera network becomes a tool for living players: lots of information, at a risk. The CCTV chore moves into it (Ethan), and the desk is useful beyond the chore.
**Touches:** `WingBuilder` (main office), a new `src/client/Controllers/CctvView.luau` (the feed UI moved out of SpectateController so both use it), a new `src/server/Services/SecurityDeskService.luau`, `BlackoutService`.

Build:
1. In the main office: a bank of monitors and a chair (a realistic asset, never low-poly), with a "Use the security desk" prompt. Sitting locks you in place like a minigame and opens a **camera map**: the wing's layout with every camera as a dot. Click a dot to view it.
2. **Ping:** on a feed, click to mark a spot. Living teammates see a marker for 6 s and hear a radio blip. 10 s cooldown.
3. **Risk:** you can't see behind you, and leaving takes about 1 s. While you sit, the Closer's normal "spotted someone" logic applies to you.
4. A **blackout** kills the desk until the breaker is reset. The Closer at a lens makes that feed shake and cut out (reuse the CloserSkin strike from the CCTV chore).
5. While sitting, `ReplicationFocus` = the camera being viewed.
6. **The CCTV chore becomes part of the desk** (Ethan): "Watch the security cameras" is done here, on the real feeds. The existing "report movement" task runs at the desk, and its fake events play over the real camera views.
7. **What the desk does beyond the chore:**
   - pings for teammates (above);
   - **door control:** lock one classroom door remotely for 20 s (once per wing), to cut off a chase for a friend you can see on camera;
   - **PA intercom:** a 3 s "radio" call from the desk that teammates hear (or a noise to pull the Closer somewhere on purpose, at a risk);
   - shows where every teammate is on the map.

Done when:
- [ ] A living player can find the Closer from the desk and ping it to a friend.
- [ ] A blackout shuts the desk down and the breaker brings it back.
- [ ] Feeds show the real world with no streaming holes.

## Phase 6: ID badges (carry your friend out)
**Goal:** being taken has stakes, and the crew has to bring everyone along. This replaces today's free return at the next wing.
**Now:** `WingService.buildWing` sets `Taken = false` and calls `LoadCharacter` for every player at every new wing, so taken players always come back.
**Rule (Ethan, 2026-11-10):** nobody can pick up the wing key while a badge is lying on the floor.
**Touches:** `src/shared/Items.luau`, `src/server/Services/WingService.luau`, `InventoryService.luau`, `AfterDeathService.luau`, `SpectateController.luau`, `HudController.luau`, [[Design/After Death]].

Build:
1. New hotbar item `IdBadge`: a lanyard with a photo ID card, data `{ Owners = { userId, ... } }`. Use a realistic model (Creator Store, or Ethan imports one), never low-poly. **Badges stack in one hotbar slot** (the lanyard holds several cards). Without stacking, a lone survivor of a 4-player crew would have no free slot for the key.
2. When someone is taken (both the Closer and the Schooler go through `WingService.onTaken`), drop their badge on the floor where they fell. It has a soft glint that only living teammates see and a label: "Sam's badge".
3. Prompt "Take Sam's badge" → `InventoryService.Give` (it joins the stack if you already carry one). If the carrier is taken, every badge they carry drops where they fell (`InventoryService.OnLost`, like the wing key). If the badge's *owner* leaves the server, their badge is removed so it can't block anything.
4. **Key gate:** the wing key's pickup checks for badges lying on the floor. If there are any, the prompt says "Bring everyone's badge first" and the objective line becomes "Sam's badge is still on the floor (back hall)". Once all badges are carried, the key works as now.
5. In `advance()`, before the next wing is built: every badge held by a living player marks its owners `ReturnNextWing`.
6. In `buildWing`'s respawn loop, only respawn players who weren't taken or who have `ReturnNextWing`. Clear the flags after.
7. **Lost & Found** (taken after the key is out, badge left behind): the badge doesn't block the door. When the next wing is built, it spawns in a **Lost & Found** bin (a realistic bin of lost items in the main office, with a hall sign pointing to it) and blocks that wing's key like any floor badge. Picking it up respawns its owner straight away in the custodian room with a flashlight (the same path the revive uses), and teammates hear "Sam is back." Until then the owner spectates and sees "Your badge is in the Lost & Found." A badge left behind in wing 5 doesn't matter: everyone shows up at dawn.
8. The R$25 revive stays as the fast way back. Reviving removes your badge from the world, which unblocks the key.
9. Everyone taken = wipe, as now. Solo has no badges. At dawn, everyone shows up in the ending shot, taken or not.
10. Spectate and HUD: the taken player sees "Your badge: on the floor in the back hall" or "Sam is carrying it". Living players' HUD lists badges still on the floor.

Done when:
- [ ] In a 2-player test: A is taken, and B can't take the key until B picks up A's badge. After the exit, A is back.
- [ ] The carrier is taken: the badge drops where they fell and blocks the key again.
- [ ] Taken after the key pickup, with the badge left behind: the badge is in the next wing's Lost & Found, it blocks the key, and picking it up brings A back immediately.
- [ ] A lone survivor can carry 3 badges plus the key.
- [ ] [[Design/After Death]] updated.

## Phase 7: School Rules system + Rule 1 (Roll Call)
**Goal:** the system every rule runs on, plus the first rule.
**Touches:** a new `src/server/Services/RuleService.luau`, new `src/shared/Rules.luau`, new `src/client/Controllers/RuleController.luau`, `JournalBook.luau`, `DevService` / `DevMenu`, `PAService`, `Config.luau`, voice lines.

Build:
1. `Rules.luau`: one entry per rule (Id, Name, Cue, Response, journal text, and 3 death hints from vague to exact).
2. `RuleService`: `Config.Rules.ByWing = { "RollCall", "LightsOut", "EyesFront", "QuietHours", "NewHire" }`. A per-wing scheduler: grace ~90 s, then the headline rule every 60–120 s and earlier rules at about a third of that rate. It skips during hunts, the intro, the tutorial, revive grace and dawn, with one rule event at a time. `NewWing` and `Clear` work like the other services.
3. `RuleController` (client): plays the cues (PA voice, tones, `LightFx.push`). No explanation text.
4. **Death hints (Doors-style):** when the Closer takes someone during a hunt a rule started, or a rule takes them directly, tag the take with the rule (`TakenBy = "LightsOut"`, set in `CloserService` / `RuleService`). The death screen (`SpectateController`, before spectating starts) shows that rule's hint. Count deaths per rule in the player's profile (`PlayerData`): 1st death → vague hint, 2nd → clearer, 3rd+ → exact. This happens on every death.
5. **Rule Book** pages in the journal: the name once you've met a rule, the plain hint once it has taken you. Add the "Journal updated" cue: the death hint ends with "Written in your journal"; on your next spawn, a page-turn sound, a **JOURNAL UPDATED · <rule>** card, the journal key hint pulsing, and a red tab with a NEW stamp on the page until it's read (`JournalBook`, `HudController`). Save read/unread per account.
6. **Roll Call:** PA chime, then the broken count from 10 to 0 (reuse TheCount's voice style). At zero, every living player who isn't hidden (`HideService.IsHidden`) is exposed. If the Closer is outside, it arrives at the nearest legal entry point (never within 40 studs of anyone or in view) and hunts the nearest exposed player. If nobody is exposed, menace drops 10.
7. Dev menu: "Fire rule ▸" with one button per rule, plus "reset my rule deaths".
8. New PA lines through `tools/make_voices.py`; Ethan uploads them and the ids go in `Config.Sounds`.

Done when:
- [ ] Roll Call fires in wing 1 after the tutorial with no explanation, and being taken by it shows the right hint (vague → exact over three deaths).
- [ ] Hiding keeps you safe; being exposed starts a hunt.
- [ ] The dev menu can fire it.
- [ ] A new note `Systems/Rules.md` explains the system.

## Phase 8: Rules 2 and 3 (Lights-Out Drill, Eyes Front)
**Touches:** `RuleService`, `RuleController`, `Rules.luau`, `LightFx`, `NoiseService`, the StillStudent scare code, `CloserService` (hunt hook).

Build:
1. **Lights-Out Drill:** a drill tone, then the hall lights turn red (`LightFx.push(light, "red")`) for 6–8 s. Anyone moving faster than ~2 studs/s or with the flashlight on gets a big `NoiseService.Emit` at their spot and is marked. If the Closer is in the building it hunts them; if not, menace jumps. The all-clear is a second tone.
2. **Eyes Front:** a still figure at a hall junction (reuse StillStudent; the faceless fallback until a Hall Monitor model exists). Each tick, check whether any living player has it on screen (in their view and not blocked) within range. If nobody does, it moves fast toward the nearest player. Reaching someone "writes them up": frozen 3 s, plus a whistle that is a noise at their spot. It lasts 60–90 s, then is gone. Only one at a time.
3. Hook both into the Rule Book and the dev menu.

Done when:
- [ ] Both rules fire in their wings and stack correctly in later wings.
- [ ] In a 2-player test, "one watches, one works" handles Eyes Front.

## Phase 9: Rules 4 and 5 (Quiet Hours, The New Hire) + stacking
**Touches:** `RuleService`, `NoiseService`, `ChoreService` noise sources, `ScareUtil` / the OwnAvatar avatar-copy code, `CloserService`.

Build:
1. **Quiet Hours:** a chime, then 30–45 s of a ticking clock. During it, loud noise sources (vacuum, plunger, sprinting, chairs, the trash chute, dropped items) are multiplied about 3× in range and start an instant hunt on whoever made them. The broom and walking are safe. A bell ends it.
2. **The New Hire (finale):** at wing start (and again if it's lost), one extra "janitor" appears. It copies a random teammate's avatar (yourself when solo), walks around and pretends to do chores. Its tells: no flashlight beam, a flickering name tag, too-smooth walking, and it never uses chat. Within ~6 studs it lunges: a jumpscare (reuse the FakeCloser-style client scare), sanity −30, it vanishes, and the Closer is called in to hunt that player at once (forced arrival if it's outside, same as Roll Call). It never takes anyone itself. Dripping and glass clinks when it's near.
3. **Stacking tune:** play wing 5 with all five rules on. If it's chaos, lower the old-rule rate.

Done when:
- [ ] Both rules work solo and with 2+ players.
- [ ] Wing 5 is hard but readable: rules never overlap and every cue is distinct.

## Phase 10: Wing atmosphere (each wing feels different, same rooms)
**Goal:** the five wings feel different without new room types or moving any chores (Ethan, 2026-11-10: no wing themes).
**Touches:** `src/server/Services/WingBuilder.luau`, `Props.luau`, `Config.luau` (new `Config.WingLooks`), `LightFx` / lighting config.

Build:
1. **Hall dressing per wing:** locker colour, banners, poster set, trophy cases or student art walls, a floor accent colour (the `Surfaces` tint), bulletin boards. No change to which rooms exist.
2. **Light per wing:** hall light colour and brightness, more dead or dying lights each wing, amber emergency lights in later wings, and the window sky going from black → deep blue → a first dawn glow in wing 5.
3. **Layout per wing:** fewer junctions in wing 1 (8–9) growing to 12–13 in wing 5; more loops later.
4. **Mess builds up:** more papers, knocked chairs, open lockers and graffiti each wing.
5. Realistic assets only ([[Systems/Assets]]); keep within the [[Systems/Performance]] budget.

Scrapped (Ethan): a name per wing and a per-wing room mix.

Done when:
- [ ] Screenshots of wings 1–5 side by side look clearly different.
- [ ] FPS is no worse than before.
- [ ] [[Systems/Map]] updated.

## Phase 11: Wing conditions (random modifiers per run)
**Goal:** the same wing plays differently from run to run.
**Touches:** `WingBuilder`, a new `src/server/Services/ConditionService.luau`, `NoiseService`, `BlackoutService`, `PAService`, `Config.Conditions`.

Build: from wing 2, each wing rolls 0 or 1 condition from a weighted pool, and the PA announces it at the start:
- **Burst pipe:** one hall section flooded; splashing footsteps are noise; spills spawn there.
- **Power rationing:** half the hall lights dead, more batteries.
- **Renovation:** plastic sheeting and scaffolding block 1–2 hall segments, which changes the route (check that pathfinding still reaches every room).
- **Alarm strobe:** a fire-alarm strobe and bell in one area that masks sound (hearing range down there, for everyone).
- **Chemical spill** (science wing): low fog in one zone.
- **Event night:** science-fair tables or dance decorations in halls (dressing only, cheap).

Done when:
- [ ] Across 10 seeds every condition shows up.
- [ ] Pathfinding reaches every hall point and every room door.
- [ ] Each condition is clear from its PA line.

## Phase 12: Light you can live with (batteries + a dead-flashlight fallback)
**Goal:** batteries stay worth buying, but an empty flashlight never leaves a player stuck in total darkness.
**Now:** a battery lasts 150 s; only 2 batteries per wing lie around (+1 per 2 extra players, so 3 for a crew of 4); the charger chore gives 2 spares for the whole crew; the dark "eyes adjust" glow is tiny (brightness 0.25, 9 studs). The coin offer (150 coins when you're nearly flat) and the after-death "give a battery" are the paid routes.
**Touches:** `src/shared/Config.luau` (Flashlight, ChoreTuning), `ChoreService` (restockChore), `WingService` (battery placement), `src/client/Controllers/Flashlight.luau`, `Props.Battery`, `NoiseService`.

Build:
0. **Batteries at the start of every wing** (Ethan, 2026-11-12): when a wing starts, and when you come back through a carried badge, the Lost & Found or a revive, your flashlight is full and you get **+1 spare battery**, up to `MaxSpareBatteries` (3). The amount is `Config.Flashlight.StartSpares = 1`, applied where WingService's `buildWing` resets each player (next to `SanityService.Reset`). Tune it with the pickups below so batteries stay worth buying.
1. **Charger = one battery per player present** (Ethan's ask). When the charger is stocked, it holds as many spares as there are players in the server at that moment (taken players count, so they get one when they're back). Each player can take **one**, so nobody hoards the crew's batteries. The prompt says "Take your battery (3 left)".
2. **Batteries are findable:**
   - more of them: `BatteriesPerWing` 2 → 3, and `BatteriesPerExtraPlayer` 0.5 → 1;
   - in places that make sense: a teacher's desk drawer, the supply shelf, beside a vending machine, instead of anywhere on a classroom floor;
   - a faint glint and a soft electric hum within ~15 studs, like the sparkle sound that leads players to Doors' books.
3. **Shake it:** with an empty battery, pressing the flashlight key shakes it: 3 s of a weak, flickering beam (~30% brightness), then a 6 s cooldown. Shaking rattles, a small noise the Closer can hear (Ethan: yes). You can always find your way, but it's clearly worse than a battery, so batteries stay worth buying.
4. **Eyes adjust a bit more:** once the battery is fully dead, the glow grows to about 0.35 brightness and 12 studs over ~5 s. That's enough to see walls and doorways, not enough to read the room. Tune it with the shake so a dead flashlight feels scary but playable.
5. **Keep the money side:** the 150-coin battery offer, the after-death "give a battery", and any future battery pack stay. Check that the offer still triggers (`Offers.Battery.Below = 0.2`).
6. Low graphics mode: check that shake and eye-adjust still look right (`LowEyesAdjustScale`).

Done when:
- [ ] Every player starts each wing with a full flashlight and +1 spare (never above 3).
- [ ] A 4-player crew finds about 5 batteries per wing plus 4 from the charger.
- [ ] With an empty flashlight you can still get from the far hall to the custodian room using shake + eyes adjust.
- [ ] The battery offer still appears when you're nearly flat.

## Phase 13: First-person hands (janitor gloves holding everything)
**Goal:** in first person you see hands holding what you hold. Right now items float in front of the camera on their own (`HotbarController` draws them; `FirstPersonBody` hides your arms).
**Touches:** a new `src/client/Controllers/ViewArms.luau`, `HotbarController`, `Flashlight`, `JournalBook`, `ToolAnim`, `ViewSway`, `Hiding`, a new arms asset in `ReplicatedStorage.Assets`, [[Systems/Inventory]].

Build:
1. **The arms:** a pair of first-person arms in a navy janitor work shirt with work gloves. **Claude builds them in Blender** (through the Blender connection), realistic and rigged (bones for the upper arm, forearm, hand and finger groups), never low-poly; Ethan approves before import. **Glove colours (Ethan):** blue, black, red and yellow. Each player in a crew gets a different colour (the glove material is recoloured per player), so you can tell whose hands are whose in clips.
2. `ViewArms` (client) attaches the arms to the camera every frame and blends between **grip poses**, one per thing you hold:
   - flashlight in the left hand, held out;
   - journal open in both hands;
   - each hotbar item: mop, broom, vacuum wand, plunger, trash bag, light tube, battery box, wing key, ID badge lanyard;
   - gadgets: one generic grip.
   When the flashlight and an item are both out, the flashlight stays in the left hand and the item goes in the right.
3. Hook up the existing motion:
   - ToolAnim's view offsets (sweeping, plunging, mopping) move the arms;
   - ViewSway and walk bob move them too;
   - the R reload gets a short battery-swap animation;
   - the Phase 12 shake gets a shake animation.
4. The flashlight beam comes from the lens of the flashlight in the gloved hand (the local light already comes from the view model).
5. Hide the arms while hiding, during cinematics, the jumpscare, spectate and the Security Desk. Never show them to other players: they see your avatar's own arms holding the item, as now.
6. Other players don't see the viewmodel, but the glove colour is also put on your avatar's hands so the crew recognises you.

Done when:
- [ ] Every hotbar item, the flashlight and the journal are held by gloved hands.
- [ ] Tool animations and the reload look right.
- [ ] Nothing clips through the camera at the Phase 3 eye height.
- [ ] Hands are gone while hiding, in cinematics and while spectating.

## Phase 14: Custodian room tools + item fixes
**Goal:** players can find the right tool straight away, and every item looks right on the floor, in the hand and in the hotbar.
**Bugs (reported by Ethan, 2026-11-10):**
- **The vacuum sinks into the floor.** `Items.Vacuum.Floor = CFrame.identity`, so a vacuum placed or dropped on the floor sits with its pivot at floor level and half the model ends up under it.
- **The trash bag is a black circle in the hotbar.** It isn't a model at all: `Props.TrashBag` is a black plastic Ball part, so the hotbar's 3D preview (ViewportFrame) shows a black circle. It looks the same in the trash can and in your hand.
- **Tools are hard to find.** The custodian room is full of clutter, which is good for the look but hides the tools.

**Touches:** `src/shared/Items.luau`, `src/shared/Props.luau`, `src/server/Services/InventoryService.luau`, `WingBuilder` (custodian room), `src/client/Controllers/HotbarController.luau`, `ChoreService` (tool spots), [[Systems/Assets]], [[Systems/Inventory]].

Build:
1. **Floor offsets from the model:** when an item is laid on the floor (spawned in the custodian room or dropped), place it from its bounding box (bottom at floor height) instead of a hand-typed `Floor` CFrame. Check every item: vacuum, broom, mop, plunger, bulb, battery box, trash bag, keys, badge.
2. **A real trash bag:** a realistic tied black bin bag model (Creator Store or Ethan imports, never low-poly), as `Assets.TrashBag`. `Props.TrashBag` uses it in the can and in the hand, and the hotbar preview frames it properly. Keep the ball only as a fallback when the asset is missing.
3. **A tool area** (Ethan): one clearly labelled area of the custodian room that still matches the scenery (a painted **TOOLS** sign / stencilled wall, a strip of hazard tape on the floor). Anything that can hang **hangs on a wall rack**: mops, brooms, the plunger. Things that can't, like the vacuum, stand on the floor in a marked spot underneath. The tube box sits on the shelf right next to it, away from the decorative clutter.
4. **Highlight what's needed:** a tool a chore on the list needs (`ToolAt` on the entry) gets a soft outline glow and a small floating icon while that chore is undone. Clutter never glows. The highlight shows while the chore is undone and the tool is still on the station.
5. Make sure decorative props never use the same models as the real tools (no fake mops next to the real one).

Done when:
- [ ] No item clips into the floor when spawned or dropped (check all of them).
- [ ] The trash bag looks like a trash bag in the can, in the hand and in the hotbar.
- [ ] A new player finds the mop within a few seconds of entering the custodian room.

## Phase 15: Close calls I: The Interrogation
**Goal:** Ethan's grab-and-interrogate close call (design in Close calls above).
**Touches:** `src/server/Services/CloserService.luau` (catch → interrogation, vent travel), a new `src/client/Controllers/InterrogationController.luau` (lifted camera, timer, name choices), `Remotes`, `Voice` / `Config.Sounds` (new creepy lines), `SanityService`, `HideService` (locker skill check on the friend), `DevService`.

Build:
1. In CloserService's catch: if the victim has living teammates, isn't last alive, and hasn't been interrogated this wing, start the interrogation instead of the take. Freeze them, attach them to its hand (the CloserFx grab pose), and pause hunting.
2. The client shows the timer (**6 s**), the friends' names and an **"I won't tell"** button, with the creepy voice line and a heartbeat. The server validates the answer: a living teammate, in time, once.
3. **Gave a name:** drop the victim (sanity −25, 2 s stun). The Closer plays "Good.", walks to the nearest vent (the vents are already entry points), climbs in, comes out of the vent nearest the named friend and **hunts them** at chase speed. The friend survives only by being in a locker or under a desk; it searches there, and the locker skill check (or the desk breath check) decides. Out in the open, it finds them.
4. **"I won't tell", or the timer ran out:** "Liar." and the normal take (badge drops).
5. New creepy voice lines through `tools/make_voices.py`; Ethan uploads them and the ids go in `Config.Sounds`.
6. A death-screen hint ("It asked about your friends. Some janitors talked."), a Rule Book-style journal note, and a dev menu "Interrogate me" button.

Done when:
- [ ] In a 2-player test: A is caught and names B; A lives, and the Closer comes out of the vent nearest B and hunts B. B in a locker gets the skill check; B in the open is taken.
- [ ] A presses "I won't tell": A is taken at once.
- [ ] Solo, or the last player alive: a normal catch.
- [ ] It happens at most once per player per wing.

## Phase 16: Close calls II
**Goal:** the other close calls (Close calls section): hold your breath at desks, hold the door, flashlight stare-down and the last-second dodge (Ethan: build all four). Build them one at a time and playtest each before the next.
**Touches:** `HideService` / `Hiding` (breath meter), `DoorService` (hold the door), `FlashlightService` / `Flashlight` (stare-down timing), `CloserService` (reactions), `SkillCheck` (reuse the locker skill-check UI).

Done when:
- [ ] Each one can save a player once per wing, has a sound tell and a cost, and looks clear on screen.
- [ ] Hold the door: the 5 s mash gets harder each wing.

## Phase 17: The lobby showpiece
**Goal:** the break room builds dread and makes players want to start a shift (Ethan: "the lobby doesn't feel like anything"). Another game keeps its monster trapped in a container; ours has to be our own idea.
**Touches:** `src/lobby/server/BreakRoom.luau`, `ShiftDoors.luau`, `src/lobby/client/LobbyClient.client.luau`, a lobby copy of the Closer model and CloserAnims, lobby sounds.

Idea (default, Ethan can change it): **"The Night Custodian, on the clock."** Behind a wired-glass window in the break room's back wall is the boiler room. The Closer stands in it, chained by the ankle to the boiler, mop in hand, mopping the same spot over and over.
- **It notices you:** its head slowly tracks the nearest player. Stand at the glass too long and it stops mopping and stares; now and then it lunges and the chain snaps tight (screen shake, chain clank, the lights over the window flicker).
- **It reacts to shifts:** when a crew's NIGHT SHIFT door starts its countdown, it drops the mop, turns toward that door and rattles the chain harder as the count runs down. When the crew leaves, it goes back to mopping. "It knows who's coming."
- **The boiler-room PA clock** shows how long until "lights out", with lore lines from the Principal on the speaker (existing voice pack).
- **A dawn tally on the window:** chalk tally marks of crews who made it to dawn (reads the existing clears leaderboard), and scratched marks for crews it took.
- **Cheap extras:** a flickering vending machine, the lore wall and gear shelf (already there) moved so the window is the first thing you see on spawn.
- Realistic assets only; the Closer here is a visual-only copy (no AI, no kill).

Done when:
- [ ] On spawn in the lobby, the trapped Closer is the first thing you see, and it reacts to players and to door countdowns.
- [ ] Lobby FPS stays fine on mobile.

## Phase 18: Tutorial, text and a full-run test
**Touches:** `TutorialService` / `TutorialController`, voice lines, the store page, the vault notes.

Build:
1. Tutorial cards and voice: shared chores, badges (the key waits until everyone's badge is picked up; the Lost & Found), the Security Desk, and shaking a dead flashlight. It doesn't explain the rules; players learn those from the cues and the death hints, as in Doors.
2. If renamed: store description and thumbnails.
3. Update [[Design/Core Loop]], [[Design/Decisions]], [[Design/Setting]], [[Design/Monsters]], [[Design/After Death]], [[Systems/Chores]] and [[Systems/Map]], so nothing still says 8 wings, owned chores or a free return.
4. Full runs with 1, 2 and 4 players. Target 40–50 min. Check how often people buy the revive vs. carry badges, and whether taken players stay in the server.

Done when:
- [ ] A 4-player run reaches dawn in about 40–50 min with no new console errors.
- [ ] Every note matches the game.

---

## Open questions for Ethan
All 21 answered on 2026-11-10 (folded into Decisions and the phases above). New ones go here.

## Found while building
(Things a phase turned up that belong to another phase or to new work.)

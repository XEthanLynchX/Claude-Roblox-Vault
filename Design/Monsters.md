# Monsters

**2026-11-10 (Ethan):** the old plan of 8 signature monsters, one per wing, is replaced. Each **School Rule** ([[Design/Redesign Plan]]) starts with a stand-in built from existing systems, and gets its own monster later. Only the five monsters tied to the rules are kept below; the Mascot, Conductor, Nurse and the backlog ideas were removed. The **Closer** roams every wing. The Schooler is being removed (broken).

Design rules for all of them:
- One clear rule per monster, learnable in one encounter, and a **counterplay** that works better in co-op.
- A **sound tell** before it acts (players should hear it before they see it).
- Built for the existing systems: private scares, sanity, blackouts, flashlight, chores.
- When a monster replaces its rule's stand-in, the rule's cue, response and consequence stay the same ([[Design/Redesign Plan]] → The School Rules); the monster may adjust its own details below to fit.
- **Model brief convention** (so the code can rig them like the Closer): separate parts named by body area, `_L`/`_R` suffixes, e.g. `torso`, `neck`, `skull`, `shoulder_L`, `upper_arm_mesh_L`, `elbow_1_L`, `forearm_mesh_L`, `hand_L`, `leg_L`, `boot_L`. Front faces +Z, feet at Y=0. Export **GLB**.

---

## The Principal · Rule 1 Roll Call (Wing 1)
> As Roll Call's monster, the Principal is the voice that does the countdown on the PA, and at zero it comes for anyone not hidden. The notes below were written when it was the wing 8 finale boss; keep the look and the PA travel, drop the finale parts.
**Rule:** **The voice from the intercom was never human.** Final wing, boss-style.
- The friendly Principal from the intro is revealed. The Principal **travels through the PA speakers**: when an intercom crackles, the Principal can step out of the nearest speaker.
- Calls **lockdowns** over the PA: specific doors lock and players get split up. The final objective is holding the front doors open together until dawn.
- **Counterplay:** move as a group, avoid rooms with speakers when the PA crackles, cut speaker wires as a chore to shrink the Principal's reach.
- **Sound tell:** microphone feedback squeal, then "Good morning, staff."
- **Model:** a tall figure in an oversized brown suit and loosened tie. Its head is a **wall-mounted PA speaker box**, with a mesh grille where the face should be and a red ON AIR light. The tie is a coiled microphone cord. A WORLD'S BEST PRINCIPAL mug in one hand, a ring of every key in the school in the other.

---

## The Overdue · Rule 2 Lights-Out Drill (Wing 2)
**Rule:** **Red light, green light.** When the library lamps flicker, freeze.
- Wanders the stacks unseen. When the green banker's lamps start flickering, it's coming: any player who **moves or has their flashlight on** during the flicker is marked and hunted.
- Standing still in the dark is safe, but your sanity drains while you wait.
- **Counterplay:** call out "FREEZE" in chat or voice; everyone has to trust each other to stay still.
- **Sound tell:** a long "Shhhhhh" through the stacks, and pages flipping by themselves.
- **Model:** a tall, gaunt librarian whose body is made of **stacked old books** under a moth-eaten cardigan. Half-moon glasses on a face of yellowed paper with typed text running across it. Long fingers like bookmarks and a card catalog drawer for a ribcage. Keys on a beaded chain.

## The Hall Monitor · Rule 3 Eyes Front (Wing 3)
**Rule:** Moves only when **nobody** is looking at it. Weeping-angel style.
- Stands at hallway junctions with a clipboard. Every time no player has it on screen, it closes distance fast.
- If it reaches you, it **writes you up**: you're frozen for 5 s and a whistle blows, telling the Closer exactly where you are.
- **Counterplay:** one player watches it while the others do chores. Blackouts and flashlight reloads are the danger moments.
- **Sound tell:** a pencil scratching on paper when it moves (only when you're not looking).
- **Model:** a tall, too-thin student shaped like a ball-jointed porcelain doll. Fluorescent orange HALL MONITOR sash, cracked glossy white face with a smile drawn on in marker, empty pencil-dot eyes. Clipboard in one hand, a whistle on a lanyard. Joints show visible seams. Stands perfectly upright, head always slightly tilted.

## The Lunch Lady · Rule 4 Quiet Hours (Wing 4)
**Rule:** **Blind, hunts by sound.**
- Patrols the kitchen and cafeteria. Sprinting, dropping trash bags, slamming doors and mopping (loud chores!) pull her toward the noise.
- Walking slowly is silent. Chores here are risky because they make noise: someone has to lure her away.
- **Counterplay:** a teammate makes noise on purpose on the other side of the room (throw a tray / bang a locker) as a distraction.
- **Sound tell:** a ladle tapping the rim of a pot in a slow rhythm that speeds up when she hears something.
- **Model:** a heavyset figure in a stained apron and hairnet, but the hairnet is pulled down over her **whole face** like a sack, with no features underneath. Rubber gloves, one arm ending in a giant steel ladle. Pushes a squeaky steel food cart full of grey slop. Hunched, sniffing.

## The Specimen · Rule 5 The New Hire (Wing 5)
**Rule:** **Mimic.** It takes the form of one of your teammates.
- Escapes from a broken jar in the science lab and copies a random teammate's avatar. It wanders and does fake chores like a real player.
- **Tells:** it has no flashlight beam, its name tag flickers, it walks slightly too smoothly, and it never uses chat.
- Get close and it reveals itself and attacks. In solo, it mimics *you*.
- **Counterplay:** check on each other ("say something if that's you"). This builds directly on the "don't trust your friends" core idea.
- **Sound tell:** a wet dripping and the clink of glass when it's near.
- **Model (true form):** a figure in a stained lab coat whose head is a **cracked specimen jar** full of murky green formaldehyde. Something pale and curled floats inside and occasionally opens one eye. Liquid drips from the coat sleeves. Rubber lab gloves, safety goggles strapped around the jar.

---

## Build order
In rule order (1 → 5), after the rule's stand-in has been playtested.

# Monsters

The **Closer** roams every wing. Each of the 8 wings also has its **own signature monster** with a unique rule to learn. New wing = new rule, which keeps runs fresh and gives every update a headline.

Design rules for all of them:
- One clear rule per monster, learnable in one encounter, and a **counterplay** that works better in co-op.
- A **sound tell** before it acts (players should hear it before they see it).
- Built for the existing systems: private scares, sanity, blackouts, flashlight, chores.
- **Model brief convention** (so the code can rig them like the Closer): separate parts named by body area, `_L`/`_R` suffixes, e.g. `torso`, `neck`, `skull`, `shoulder_L`, `upper_arm_mesh_L`, `elbow_1_L`, `forearm_mesh_L`, `hand_L`, `leg_L`, `boot_L`. Front faces +Z, feet at Y=0. Export **GLB**.

---

## 1. The Hall Monitor (Wing 1 · Main Hall)
**Rule:** Moves only when **nobody** is looking at it. Weeping-angel style.
- Stands at hallway junctions with a clipboard. Every time no player has it on screen, it closes distance fast.
- If it reaches you, it **writes you up**: you're frozen for 5 s and a whistle blows, telling the Closer exactly where you are.
- **Counterplay:** one player watches it while the others do chores. Blackouts and flashlight reloads are the danger moments.
- **Sound tell:** a pencil scratching on paper when it moves (only when you're not looking).
- **Model:** a tall, too-thin student shaped like a ball-jointed porcelain doll. Fluorescent orange HALL MONITOR sash, cracked glossy white face with a smile drawn on in marker, empty pencil-dot eyes. Clipboard in one hand, a whistle on a lanyard. Joints show visible seams. Stands perfectly upright, head always slightly tilted.

## 2. The Mascot (Wing 2 · Gym)
**Rule:** The **opposite** of the Hall Monitor: if you **look at it**, it chases you.
- Sits slumped on the bleachers. Looking directly at it (centered on screen, within range) wakes it up; it charges as long as you keep looking and slows when you look away.
- Looking down at your feet or turning your back stops it. Players must do gym chores while deliberately **not looking**.
- **Counterplay:** teammates call out its position ("don't look left!"). Flashlight pointed away keeps you safe but blind.
- **Sound tell:** squeaking sneakers on the gym floor, and a faint crowd cheering whenever someone looks at it.
- **Model:** a filthy, half-deflated tiger mascot costume. The oversized head is too big for the body and sags to one side; its sewn-on felt grin is coming apart at the seams. Through the mesh eye holes you can see a wet glint. Matted fur, a #00 jersey, and a tail dragging on the floor. Arms hang past its knees.

## 3. The Lunch Lady (Wing 3 · Cafeteria)
**Rule:** **Blind, hunts by sound.**
- Patrols the kitchen and cafeteria. Sprinting, dropping trash bags, slamming doors and mopping (loud chores!) pull her toward the noise.
- Walking slowly is silent. Chores here are risky because they make noise: someone has to lure her away.
- **Counterplay:** a teammate makes noise on purpose on the other side of the room (throw a tray / bang a locker) as a distraction.
- **Sound tell:** a ladle tapping the rim of a pot in a slow rhythm that speeds up when she hears something.
- **Model:** a heavyset figure in a stained apron and hairnet, but the hairnet is pulled down over her **whole face** like a sack, with no features underneath. Rubber gloves, one arm ending in a giant steel ladle. Pushes a squeaky steel food cart full of grey slop. Hunched, sniffing.

## 4. The Overdue (Wing 4 · Library)
**Rule:** **Red light, green light.** When the library lamps flicker, freeze.
- Wanders the stacks unseen. When the green banker's lamps start flickering, it's coming: any player who **moves or has their flashlight on** during the flicker is marked and hunted.
- Standing still in the dark is safe, but your sanity drains while you wait.
- **Counterplay:** call out "FREEZE" in chat or voice; everyone has to trust each other to stay still.
- **Sound tell:** a long "Shhhhhh" through the stacks, and pages flipping by themselves.
- **Model:** a tall, gaunt librarian whose body is made of **stacked old books** under a moth-eaten cardigan. Half-moon glasses on a face of yellowed paper with typed text running across it. Long fingers like bookmarks and a card catalog drawer for a ribcage. Keys on a beaded chain.

## 5. The Specimen (Wing 5 · Science Wing)
**Rule:** **Mimic.** It takes the form of one of your teammates.
- Escapes from a broken jar in the science lab and copies a random teammate's avatar. It wanders and does fake chores like a real player.
- **Tells:** it has no flashlight beam, its name tag flickers, it walks slightly too smoothly, and it never uses chat.
- Get close and it reveals itself and attacks. In solo, it mimics *you*.
- **Counterplay:** check on each other ("say something if that's you"). This builds directly on the "don't trust your friends" core idea.
- **Sound tell:** a wet dripping and the clink of glass when it's near.
- **Model (true form):** a figure in a stained lab coat whose head is a **cracked specimen jar** full of murky green formaldehyde. Something pale and curled floats inside and occasionally opens one eye. Liquid drips from the coat sleeves. Rubber lab gloves, safety goggles strapped around the jar.

## 6. The Conductor (Wing 6 · Music & Auditorium)
**Rule:** **Musical statues.** While the music plays you're safe; when it stops, don't move.
- An out-of-tune piano or recorder plays through the wing. When the music suddenly cuts out, anyone moving within its sight is hunted.
- The pauses get shorter and less predictable the longer the wing takes.
- **Counterplay:** do chores during the music and freeze in the silence. The fear comes from being mid-sprint when it stops.
- **Sound tell:** a ticking metronome that speeds up just before the music stops.
- **Model:** a gaunt figure in a tattered tuxedo with tails, white gloves, and a **brass metronome for a head** whose arm ticks side to side. Arms twice as long as they should be, one holding a cracked baton. Sheet music stuck all over its body.

## 7. The Nurse (Wing 7 · Nurse's Office & Basement)
**Rule:** **Only hunts players with low sanity, and only they can see her.**
- Invisible to players with sanity above ~50%. Players below that see her and are hunted.
- Healthy teammates have to guide the low-sanity player ("she's behind you!" "I can't see her!"), or bring them to the sanity pills in her office, which costs time.
- **Counterplay:** manage sanity as a team. The Wellness Monitor in the custodian room suddenly matters a lot.
- **Sound tell:** the squeak of an IV stand's wheels and a hummed lullaby.
- **Model:** an old-fashioned nurse's uniform, yellowed and stained, with a white cap. A surgical mask is **stitched onto her face** and there are no eyes above it. She drags a rusty IV stand with a bag of something dark, and carries a tray of pills that rattles. Long cracked fingernails.

## 8. The Principal (Wing 8 · Principal's Office · Finale)
**Rule:** **The voice from the intercom was never human.** Final wing, boss-style.
- The friendly Principal from the intro is revealed. The Principal **travels through the PA speakers**: when an intercom crackles, the Principal can step out of the nearest speaker.
- Calls **lockdowns** over the PA: specific doors lock and players get split up. The final objective is holding the front doors open together until dawn.
- **Counterplay:** move as a group, avoid rooms with speakers when the PA crackles, cut speaker wires as a chore to shrink the Principal's reach.
- **Sound tell:** microphone feedback squeal, then "Good morning, staff."
- **Model:** a tall figure in an oversized brown suit and loosened tie. Its head is a **wall-mounted PA speaker box**, with a mesh grille where the face should be and a red ON AIR light. The tie is a coiled microphone cord. A WORLD'S BEST PRINCIPAL mug in one hand, a ring of every key in the school in the other.

---

## Other ideas (backlog)
- **The Substitute:** sits at a desk facing the board. Do chores in its room with the lights on; never be in there during a blackout.
- **Things That Follow:** mop buckets or rolling chairs that are closer every time you look back (environmental, any wing).
- **Late Bus Kid:** crying from a bathroom stall. Comfort it for sanity, unless your sanity is low.
- **Lost & Found:** the backpacks on the lost & found table belong to the missing janitors, and one of them is breathing.

## Build order suggestion
1. Hall Monitor (cheap, great co-op, uses only line-of-sight checks)
2. Mascot (same code as Hall Monitor, inverted)
3. Lunch Lady (adds a noise system the Closer can also use)
4. Specimen (reuses avatar copying; core theme)
5. Overdue, Conductor (timed global events)
6. Nurse (sanity-gated visibility)
7. Principal (finale; needs PA speakers in the map)

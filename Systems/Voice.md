# Voice lines

Every spoken line has audio. One Piper TTS voice (LibriTTS speaker 58) for everything; `tools/make_voices.py` (replaced `make_pa.py`) renders and packs them. Each pack is ONE uploaded file; lines are time ranges in it.

| Pack | File / Config.Sounds key | Lines module | What |
|---|---|---|---|
| PA | `sounds/PAVoice.ogg` / `PAVoice` | `PALines` | the 17 school PA announcements (PAService). Only rebuilt with `--pa` |
| Extra | `sounds/PAVoiceExtra.ogg` / `PAVoiceExtra` | `PAExtraLines` | private PA scare normal lines (PA chain) + the phone caller's normal lines (telephone chain) |
| Creepy | `sounds/CreepyVoice.ogg` / `CreepyVoice` | `CreepyLines` | creepy lines: same voice slowed/lowered, doubled by a drifting second voice, whisper layer, stutter, bit-crush bursts, dropouts, a reversed fragment (phone ones also through the telephone line) |

- Play: `Shared/Voice.luau` -> `Voice.play(name, parent?, volume?)`, `Voice.get`, `Voice.find(text)` (phone questions are matched by their exact text, so changing a question's text in PhoneQuestions needs a re-render), `Voice.isCreepy`.
- Normal vs creepy: normal = PA voice; creepy = anything the school says that is wrong ("...we can see you.", "Your shift is almost over.", the 6 creepy phone questions, "...Wrong.").
- Phone: "Night staff? This is the front office." then the question; "...Good. Get back to work." / "...Wrong." outlive the phone window.
- Private PA scare (`Scares/PAWhisper`): chime, then the line; the subtitle types out over the spoken part; creepy lines tinted red and jittering. "N people remaining" always says one more than the players (2-5 voiced).
- Piper isn't deterministic: re-rendering shifts every time range, so a re-render means re-uploading that pack. That's why the old PA file stays untouched.
- Rendering runs in Claude's cloud workspace (Piper + the voice model aren't on Ethan's PC): `pip install piper-tts numpy`, model from the piper GitHub release (see the script header).
- Not voiced on purpose: tutorial cards, notes, notifications, graffiti tags (e.g. "WE SEE YOU" is a graffiti tag, not speech).

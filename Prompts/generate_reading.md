# generate_reading

**Purpose:** write a reading passage matched to the user's real vocabulary and grammar, then grade understanding.
**When:** in the `loop` phase, after lessons, whenever the user wants something to read.

## User inputs

- Anki export is up to date: yes / no  ({{export_status}})
- Topic or style (optional): {{topic}}
- Length (optional, default depends on level): {{length}}
- Target lessons to reinforce (optional, default = the last 2-3 checked lessons): {{target_lessons}}

## Instructions for the agent

1. Read `README.md`, `00-State.md`, the target lessons, and `Vocab/anki-export.txt`. If the export is missing or stale, ask the user to refresh it before continuing.
2. Check the passage will fit README §7: ≥ 95% of words from the user's known buckets (`mature`, `young`, `learning`), 3-5% new words, grammar only from covered lessons, with the target lessons' grammar appearing several times. Reuse `leeches` and `young` words on purpose. Length is about 80-250 words depending on `reading_level`.
3. Write the passage, a short glossary of the new words, and 4-5 comprehension questions (gist, detail, and at least one about a grammar point). Show it in chat. Mention that the user should listen to the audio (text-to-speech or a recording) alongside reading.
4. Create `Reading/R###-<slug>.md` (README §6.4) with status `generated`, and a card file for the new words (`Prompts/create_anki_cards.md`).
5. After the user reads: ask for (a) answers to the questions, (b) any other unknown words they hit, (c) a rough count of unknown words. Grade the answers and record `comprehension` as a percentage.
6. Apply the reading-level rule (README §7). Add cards for the newly reported unknown words. Add or update mistake patterns.
7. Follow README §9: change list (Reading note, card files, State reading level, Mistakes), confirm, write, verify.
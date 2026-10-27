# gate_test

**Purpose:** run a gate, decide pass or fail, and update the user's phase or flags.
**When:** when the next roadmap entry is a `type: gate` item.

## User inputs

- Gate ID: {{gate_id}}  (leave empty = the next gate entry in the roadmap)
- Anki export is up to date: yes / no  ({{export_status}})

## Instructions for the agent

1. Read `README.md` (especially §7 gates), `00-State.md`, `01-Roadmap.md`, `Mistakes.md`, all lessons since the previous gate, and `Vocab/anki-export.txt` if the gate involves vocabulary. If the export is stale, ask the user to refresh it.
2. Build the gate test from the gate's criteria in README §7. Use only content the user has actually studied. For the `familiarization` gate, build all four parts (a-d).
3. Create the test note (`Tests/G###-<name>.md`, kind `gate`) without an answer key. Administer it in chat, part by part.
4. Grade each part against its threshold from README §7. Report each part's score and pass/fail clearly.
5. **Decision:**
   - **Pass:** state it, then propose State changes (add the gate to `gates_passed`; for `familiarization` set `phase: loop`; for unlock gates set the flag to `unlocked`).
   - **Fail:** list the weak areas, propose remedial lessons to insert before a retry (and a new gate entry), and set mastery to `shaky` on the related lessons. Do not advance the phase.
   - **Second failure of the same gate:** ask the user whether to proceed anyway or add more lessons.
6. Add mistake patterns found, and card files for missed words (`Prompts/create_anki_cards.md`).
7. Follow README §9: change list, confirm, write, verify. If the gate passed, suggest the next prompt (usually `extend_roadmap.md` or `learn_lesson.md`).
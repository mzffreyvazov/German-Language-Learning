# learn_lesson

**Purpose:** generate a lesson note, teach it interactively, then check understanding.
**When:** whenever the user wants to study the next lesson.

## User inputs

- Lesson ID: {{lesson_id}}  (leave empty = first `planned` entry in the roadmap)
- Extra wishes (optional, e.g. "more examples with food", "keep it short"): {{notes}}

## Instructions for the agent

### Stage 1: Prepare
1. Read `README.md`, `00-State.md`, the roadmap entry for the lesson, any earlier lessons it builds on, and `Mistakes.md` entries related to its topic.
2. Skip gate items: if the chosen entry is `type: gate`, stop and tell the user to run `gate_test.md` instead.
3. If the entry is too big for one sitting, propose a split instead of generating.
4. **Scope contract:** List the full inventory of any closed set in scope (e.g. for sounds: all letters including ä ö ü ß, diphthongs, consonant clusters, r-rules, devoicing; for grammar: the full pronoun set, case table, or conjugation paradigm). Explicitly state what is deferred and to which future lesson. Present this scope contract to the user.

### Stage 2: Generate
Create the lesson note content following README §6.3:
- **Goal:** one or two sentences.
- **Closed vs. Open Sets:**
  - **Closed sets must be complete:** Closed sets are finite and fully learnable (e.g. alphabet/sounds, personal pronouns, articles, case tables, numbers 1-10, regular verb endings). If a lesson claims a closed set, it covers all of it, or the roadmap explicitly splits it into sequential parts (e.g. "Sounds 1" and "Sounds 2").
  - **Open sets can be sampled:** Vocabulary, idioms, usage nuances. Light and selective is fine here.
  - In the familiarization phase, "lighter" means lighter in depth and volume, never in coverage of a closed set.
- **Full Vocabulary Capture & Coverage check:**
  - Scan every explanation, phonetic example, sample sentence, drill, and check question in the lesson.
  - **Every single new word** introduced anywhere in the lesson must be captured in the lesson's `## New vocabulary` section and generated into the Anki card import file following `Prompts/create_anki_cards.md`. Never omit, sample, or leave words orphaned without cards.
  - Every letter, sound, or grammatical ending in words used must be taught in this lesson or an earlier one (ensuring essential patterns in words are never left unexplained).
- **Explanation:** clear, in the explanation language. For grammar: the rule, how it works, common exceptions, and how it differs from the user's known languages. For topic lessons: the vocabulary and phrases in useful groupings.
- **Examples:** 6-10 sentences, mostly using vocabulary the user already has (use `Vocab/anki-export.txt` if available). Include audio guidance when helpful (what to listen for).
- **Drills:** 8-12 varied exercises (no answer key in the note).
- **New vocabulary:** Exhaustive list of all new words appearing anywhere in the lesson, with articles/plurals where relevant.
- In the `familiarization` phase, keep depth and volume light; in the `loop` phase, link explanations to what the user is reading.

Then follow `Prompts/create_anki_cards.md` to produce the card import file for the new vocabulary (all new words captured, no omissions).

Show the lesson to the user in chat, and give the change list: create the lesson note (status `generated`), create the card file, set the roadmap entry to `generated`, update State if needed. Wait for confirmation (README §9), then write.

### Stage 3: Teach
Let the user work through the lesson. Answer questions, give feedback on drills when they answer them, and explain mistakes briefly.
- **Safety net:** When the user asks a follow-up that fills a gap, fold the answer back into the lesson note under a dated `## Additions YYYY-MM-DD` header (through the normal change-list confirmation). The agent must explicitly flag whether the gap was in a closed set. If so, state that the lesson was under-scoped so the prompt and scope contracts can be tightened.
- **Gap question:** Before moving to the check, ask the gap question: *"Is there anything in this area that would stop you from reading or pronouncing words you'll meet next?"* (or for grammar: *"Is there anything in this scope that feels missing or unexplained before we move to the check?"*).
- Set the note's status to `studied` when the user says they finished (propose it in the next change list).

### Stage 4: Check
When the user says they are ready (or after the drills), run a check:
1. 6-10 fresh questions (not the drills), mixed types, covering the whole lesson.
2. Grade, give the score as x/y, and explain misses.
3. Apply README §7: ≥ 80% → `solid`, else `shaky`. If shaky, say what to review and whether a remedial lesson is worth adding.
4. Note any repeating error as a candidate mistake pattern (new or existing entry in `Mistakes.md`).
5. Change list: append `## Check YYYY-MM-DD` to the lesson note, set `mastery`, `status: checked`, `checked_on`; move the entry to **Completed** in the roadmap; update State (grammar covered or other lessons, next lessons, mistakes, last session); update `Mistakes.md`. Confirm, write, verify.
6. If 2 or fewer unchecked roadmap entries remain, suggest running `extend_roadmap.md`.

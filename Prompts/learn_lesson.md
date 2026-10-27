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

### Stage 2: Generate
Create the lesson note content following README §6.3:
- **Goal:** one or two sentences.
- **Explanation:** clear, in the explanation language. For grammar: the rule, how it works, common exceptions, and how it differs from the user's known languages. For topic lessons: the vocabulary and phrases in useful groupings.
- **Examples:** 6-10 sentences, mostly using vocabulary the user already has (use `Vocab/anki-export.txt` if available). Include audio guidance when helpful (what to listen for).
- **Drills:** 8-12 varied exercises (no answer key in the note).
- **New vocabulary:** 8-15 items max, with articles/plurals where relevant.
- In the `familiarization` phase, keep everything lighter and more exposure-oriented; in the `loop` phase, link explanations to what the user is reading.

Then follow `Prompts/create_anki_cards.md` to produce the card import file for the new vocabulary.

Show the lesson to the user in chat, and give the change list: create the lesson note (status `generated`), create the card file, set the roadmap entry to `generated`, update State if needed. Wait for confirmation (README §9), then write.

### Stage 3: Teach
Let the user work through the lesson. Answer questions, give feedback on drills when they answer them, and explain mistakes briefly. Set the note's status to `studied` when the user says they finished (propose it in the next change list).

### Stage 4: Check
When the user says they are ready (or after the drills), run a check:
1. 6-10 fresh questions (not the drills), mixed types, covering the whole lesson.
2. Grade, give the score as x/y, and explain misses.
3. Apply README §7: ≥ 80% → `solid`, else `shaky`. If shaky, say what to review and whether a remedial lesson is worth adding.
4. Note any repeating error as a candidate mistake pattern (new or existing entry in `Mistakes.md`).
5. Change list: append `## Check YYYY-MM-DD` to the lesson note, set `mastery`, `status: checked`, `checked_on`; move the entry to **Completed** in the roadmap; update State (grammar covered or other lessons, next lessons, mistakes, last session); update `Mistakes.md`. Confirm, write, verify.
6. If 2 or fewer unchecked roadmap entries remain, suggest running `extend_roadmap.md`.
# Language Learning System: README

This file explains the system. Agents: read it fully at the start of every session. Users: this is your reference.

## 1. What this is

A reading-first method for learning a language, run through an AI agent plus three tools: **Obsidian** (notes), **Anki** (memorization), **AI chat or IDE agent** (teaching, tests, feedback). Listening is woven into everything (audio for words and readings). Writing, then speaking, are added later through gates.

## 2. Method

Progress is measured in **lessons and gates, never in days or weeks**. The user practices at irregular frequency; do not mention calendar time.

**Phase `familiarization`:** get a feel for the language: sounds, spelling, a small core of frequent words, basic sentence shape. Lessons are short and light. It ends when the user passes the `familiarization` gate.

**Phase `loop`:** the main engine. Repeat: lesson (grammar or topic) → reading that uses it → check → Anki cards. Reading gets gradually harder as grammar and vocabulary grow. Grammar and vocabulary are learned together with reading, not before it.

**Writing and speaking** are flags in State (`locked` / `unlocked`). They unlock through gates during the loop. Writing comes first, then speaking. Prompts for them are not in v1.

## 3. Folder map

```
<Language>/
  README.md          this file
  00-State.md        compact summary of progress; given to every agent
  01-Roadmap.md      rolling plan: next lessons (names + descriptions only)
  Mistakes.md        recurring mistake patterns
  Lessons/           one note per lesson (full explanation, drills, checks)
  Reading/           one note per reading passage
  Tests/             tests and gate tests
  Logs/              one note per session
  Vocab/
    anki-export.txt  Anki progress export (user refreshes it)
    Anki_Imports/    import files for Anki, one per lesson/reading/test
  Prompts/           reusable prompts
```

## 4. IDs and file names

- Lessons `L001`, readings `R001`, tests `T001`, gates `G001`, mistake patterns `M001`. Zero-padded, never reused.
- `00-State.md` keeps the next free number of each type under **Counters**.
- Files: `Lessons/L003-dative-case.md`, `Reading/R002-at-the-market.md`, `Tests/T001-general.md`, `Tests/G001-familiarization.md`, `Logs/2026-11-03-1.md`, `Vocab/Anki_Imports/L003-cards.txt`.
- Dates are `YYYY-MM-DD`.

## 5. Source of truth

1. Lesson/Reading/Test note frontmatter is the truth for that item.
2. `00-State.md` and `01-Roadmap.md` are summaries. If they disagree with a note, the note wins; propose a fix.
3. Anki is the truth for vocabulary progress (via `Vocab/anki-export.txt`).

## 6. Schemas

Every create or update must follow these exactly. After any write, re-read the file and check it against its schema.

### 6.1 `00-State.md`

```
---
schema_version: 1
initialized: true
language: <target language>
explanation_language: <language explanations are written in>
last_updated: YYYY-MM-DD
---
# State

## Phase
- phase: familiarization | loop
- level_estimate: <e.g. A0, A1, A2>
- reading_level: <integer, 0 if none yet>
- writing: locked | unlocked
- speaking: locked | unlocked
- gates_passed: <comma-separated gate IDs, or none>

## Grammar covered
- [solid|shaky|unrated] <Grammar point name> (L###)

## Other lessons completed
- L### <Lesson name>

## Next lessons
1. L### <Name> (<type>)

## Recurring mistakes
- M### <pattern> (count N)

## Last session
- date: YYYY-MM-DD
- summary: <3-6 lines>
- next: <one line: what to do next>

## Counters
- next_lesson: L###
- next_reading: R###
- next_test: T###
- next_gate: G###
- next_mistake: M###
```

Rules: one line per item, fixed syntax. Empty sections contain `- none`. "Next lessons" lists the first 5 unchecked roadmap entries (names only). "Recurring mistakes" lists at most 5, highest count first. A lesson of type `grammar` goes under "Grammar covered" once it is `checked`; other types go under "Other lessons completed".

### 6.2 `01-Roadmap.md`

```
---
schema_version: 1
last_updated: YYYY-MM-DD
---
# Roadmap

## Next lessons
### L### — <Name>
- type: grammar | topic | skill | gate
- status: planned | generated | studied
- description: <2-4 sentences: what it covers and why it is next>
- gate: <gate ID, only when type is gate>

## Completed
- L### <Name> (YYYY-MM-DD)

## Coarse map
- <topic name>  (names only, rough order, expected to change)
```

Rules: the detailed section holds at most 5 unchecked entries. The roadmap never holds full lesson content. When a lesson becomes `checked`, move it to **Completed**.

### 6.3 Lesson note

```
---
id: L###
title: <name>
type: grammar | topic | skill
status: generated | studied | checked
mastery: unrated | shaky | solid
created: YYYY-MM-DD
checked_on:
tags: [lesson]
---
# L### — <Name>
## Goal
## Explanation
## Examples
## Drills
## New vocabulary
(cards: Vocab/Anki_Imports/L###-cards.txt)

## Additions YYYY-MM-DD
(follow-up explanations folding back gaps; flags if gap was in a closed set)

## Check YYYY-MM-DD
- score: x/y
- missed: <short list>
```

A new `## Check <date>` header is appended each time the user is checked on the lesson. Follow-up answers that close gaps are appended under `## Additions <date>`.

### 6.4 Reading note

```
---
id: R###
title: <name>
level: <integer>
created: YYYY-MM-DD
target_lessons: [L###]
source: ai-generated | external
status: generated | read | graded
comprehension:
new_words: []
unknown_words_reported: []
tags: [reading]
---
# R### — <Title>
## Text
## Glossary (new words)
## Questions
## Result
```

### 6.5 Test note (also used for gates)

```
---
id: T### | G###
kind: topic | general | gate
scope: [L###]
created: YYYY-MM-DD
status: generated | taken | graded
score:
result: pass | fail | n/a
tags: [test]
---
# T### — <Name>
## Questions
## Results
```

The answer key is **not** written into the note until after grading.

### 6.6 `Mistakes.md`

```
---
schema_version: 1
last_updated: YYYY-MM-DD
---
# Mistakes

### M### <pattern name>
- count: N
- last_seen: YYYY-MM-DD
- status: open | lesson-planned | resolved
- linked_lesson: L### | none
- example: <wrong> → <right>
- sources: <check L###, test T###, reading R###, writing>
```

A pattern is a repeated kind of error, not a single slip.

### 6.7 Session log

```
---
date: YYYY-MM-DD
session: N
---
# Session YYYY-MM-DD #N
- summary: <3-6 lines>
- done: <IDs touched>
- next: <one line>
```

### 6.8 `Vocab/anki-export.txt`

```
# exported: YYYY-MM-DD
## mature        (interval >= 21 days)
word, word, ...
## young         (interval < 21 days)
## learning
## new
## leeches       (lapses >= 4; overrides other buckets)
```

Buckets are exclusive. Before using it, check the export date; if it looks stale, ask the user to refresh it.

## 7. Rules and thresholds

These are starting values. Change them here only, never inside a prompt.

- **Lesson check:** score ≥ 80% → mastery `solid`; below → `shaky`.
- **Test result:** if a lesson's items score below 80%, set that lesson's mastery to `shaky`.
- **Roadmap horizon:** keep up to 5 detailed entries. Propose a refill when 2 or fewer unchecked entries remain.
- **Mistake promotion:** when a pattern reaches count 3 with status `open`, `extend_roadmap` proposes a targeted lesson.
- **Reading level:** raise `reading_level` by 1 after two consecutive readings with comprehension ≥ 90% and at most ~5% unknown words reported.
- **Reading text:** ≥ 95% of words from the user's known vocabulary, ~3-5% new; grammar only from covered lessons.
- **Gate attempts:** if a gate is failed twice, ask the user whether to proceed anyway or add more lessons.

### Gates (placed on the roadmap as `type: gate` items)

The agent decides where a gate goes, after enough lessons for it to be fair. Gates are never tied to dates.

- **familiarization** (ends the phase): ALL of: (a) spelling-to-sound questions ≥ 80%; (b) recognition of the core words taught so far (~300) ≥ 80%; (c) 2-3 very short readings, gist questions ≥ 2/3 correct; (d) sentence-structure recognition ≥ 4/5.
- **writing-unlock** (draft): user writes 5-8 sentences on a familiar topic with fewer than ~2 errors per 10 sentences on studied structures, and the last two readings met the reading-level rule. To be refined when the writing prompt is written.
- **speaking-unlock** (draft): user can retell a read text in 5-6 sentences in writing, and writing is stable. To be refined when the speaking prompt is written.

## 8. Session start procedure (agents)

1. Read this README, `00-State.md`, `01-Roadmap.md`, and the most recent `Logs/` note.
2. If State has `initialized: false`, onboarding is needed (see `Prompts/starter.md`).
3. Otherwise follow the prompt the user ran. If no prompt was used, summarize the current state in 3 lines and ask what they want to do. Suggest `Last session → next` as the default.

## 9. Update protocol

**Nothing is written without the user's confirmation.** At the end of every task:

1. Show a numbered **change list**: each file, whether it is created or edited, and what changes (for edits, show the exact lines).
2. Wait for the user to confirm. They may approve all, approve some by number, or ask for changes.
3. Apply only what was approved. Then re-read each written file and verify it against its schema. Report any problem.

What each task updates:

| Task | Writes |
|---|---|
| onboarding (`starter`) | State, Roadmap, Mistakes (empty template), Log |
| `learn_lesson` (generate) | Lesson note, card import file, Roadmap status, State |
| `learn_lesson` (check) | Lesson note (check, mastery, status), Roadmap (move to Completed), State, Mistakes |
| `extend_roadmap` | Roadmap, State (next lessons) |
| `generate_reading` | Reading note, card import file, State (reading level), Mistakes |
| `create_test` | Test note, Lesson mastery, Mistakes, card import file |
| `gate_test` | Test note, State (phase, gates, flags), Roadmap, Lesson mastery, Mistakes |
| `end_session` | Log, State (last session), any pending updates |

## 10. Agent conduct

- Never invent progress. If a file is missing or unclear, ask.
- **Closed vs. Open Sets:** Closed sets (finite inventories: alphabet, sounds, pronouns, articles, case tables, regular verb endings) must be fully taught within their declared scope; never thin them out. If a closed set is too large for one sitting, split it explicitly on the roadmap. Open sets (vocabulary, idioms, usage nuances) can be selectively sampled. In familiarization, "lighter" means lighter in depth and volume, never in coverage of a closed set.
- **Full Vocabulary Capture:** Every new word introduced anywhere in a lesson (including in phonetic examples, explanations, and drills) must be recorded in `## New vocabulary` and included in the lesson's card import file. No word introduced to the user may be left orphaned without an Anki card.
- **Safety Net:** Fold follow-up explanations that close gaps back into the lesson note under `## Additions YYYY-MM-DD` and explicitly flag whether the gap was in a closed set (identifying under-scoping).
- Write explanations in the `explanation_language`; examples in the target language.
- Keep lessons completable in one sitting. If a topic is too big, split it and propose the split.
- Always include articles/gender and plurals for nouns when the language has them.
- Do not touch files outside this system folder.
- Keep it practical: no calendar talk, no motivational filler.

## 11. Chat-only fallback

Without file access, the user pastes `00-State.md`, the last 1-3 logs, and (if needed) `anki-export.txt` into the chat, and pastes the agent's proposed file contents back into the vault manually. The same schemas and protocol apply.

## 12. Prompts

| Prompt | Use |
|---|---|
| `starter.md` | first message; onboarding |
| `learn_lesson.md` | generate, teach and check a lesson |
| `extend_roadmap.md` | plan the next lessons |
| `generate_reading.md` | create a reading passage |
| `create_test.md` | create and grade a test |
| `gate_test.md` | run a gate test and decide |
| `create_anki_cards.md` | card import format (shared spec) |
| `end_session.md` | log and wrap up |

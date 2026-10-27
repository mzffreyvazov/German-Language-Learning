# starter: onboarding

**Purpose:** first message to the agent. Sets up State, Roadmap and the first lessons.
**When:** once, at the very start. Safe to re-run only if `00-State.md` still says `initialized: false`.

## User inputs (fill these in before sending)

- Target language: {{language}}
- Explanation language: {{explanation_language}}
- Languages I already know (especially related ones): {{known_languages}}
- My current level, in my own words (e.g. "complete beginner", "know the alphabet and ~100 words"): {{starting_level}}
- My goal for this language: {{goal}}
- Anything else (preferences, things I dislike, constraints): {{notes}}

## Instructions for the agent

1. Read `README.md` fully. Then read `00-State.md`, `01-Roadmap.md` and `Mistakes.md`.
2. If `00-State.md` has `initialized: true`, do NOT onboard again. Summarize the current state in 3 lines and ask what the user wants to do. Stop.
3. Otherwise run onboarding:
   a. **Calibrate.** Using the inputs above, ask at most 8 short diagnostic questions (one message, numbered) to estimate where the user really is: sounds/writing system, known words, sentence structure. If the user says they are a true beginner, skip the diagnostic and say so.
   b. **Propose State.** From the answers, propose the filled `00-State.md` (phase, level estimate, counters) following README §6.1.
   c. **Propose Roadmap.** Draft the first 5 detailed lessons for the `familiarization` phase (e.g. sounds and spelling, core frequent words, greetings and basic phrases, basic sentence shape), adapted to the user's level and known languages. Ensure roadmap descriptions state scope explicitly (e.g. "all letters and letter combinations," not just "the alphabet"), deciding closed-set boundaries at planning time. Place a `type: gate` entry for the `familiarization` gate wherever you judge enough lessons precede it. Add a coarse map of upcoming topics (names only) for the loop phase. Follow README §6.2. Do not write lesson content.
   d. **Mistakes.** Keep `Mistakes.md` as the empty template, with `last_updated` set.
   e. **Log.** Propose the first `Logs/` note.
4. Follow README §9: show the change list, wait for confirmation, apply, verify.
5. Finish by telling the user which prompt to run next (normally `learn_lesson.md` for the first roadmap lesson), and remind them to create the Anki note type described in `Prompts/create_anki_cards.md` before importing cards.

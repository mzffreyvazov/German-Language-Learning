# create_test

**Purpose:** create a midterm-style test to check retention, grade it, and feed results back.
**When:** whenever the user wants a review. Not for lesson checks (those live in `learn_lesson`) and not for gates.

## User inputs

- Scope: {{scope}}  (`general` = everything studied since the last test, or a list of lesson IDs or a topic)
- Length: {{length}}  (default 20 questions)
- Question types to include or avoid (optional): {{types}}

## Instructions for the agent

1. Read `README.md`, `00-State.md`, the lessons in scope, `Mistakes.md`, and `Vocab/anki-export.txt` if present.
2. Select content, weighted toward: `shaky` lessons, open mistake patterns, lessons not tested recently, and leech words. Do not only test what is easy.
3. Use a varied mix. Default question types (the user may edit this list):
   - multiple choice
   - fill in the blank
   - translate into the target language
   - translate into the explanation language
   - error correction (find and fix the mistake)
   - reorder words into a sentence
   - short reading with comprehension questions
   - matching
4. Create `Tests/T###-<name>.md` (README §6.5) with questions only. Keep the answer key in the conversation; do not write it into the note yet.
5. Let the user answer in chat. Grade after they finish.
6. After grading:
   - Write the results into the note, including correct answers for missed items.
   - Per lesson: if its items scored below 80%, propose `mastery: shaky`.
   - Add or update mistake patterns.
   - Create a card file for missed words (`Prompts/create_anki_cards.md`).
7. Follow README §9: change list, confirm, write, verify.
# create_anki_cards: card import format (shared spec)

**Purpose:** define exactly how vocabulary import files are produced. Other prompts (`learn_lesson`, `generate_reading`, `create_test`, `gate_test`) call this. It can also be run on its own.

## User inputs (only when run standalone)

- Words or phrases to turn into cards (or the source ID, e.g. L004): {{source}}

## One-time Anki setup (user)

Create a note type named **`Language Vocab`** with these fields, in this order:

1. `Front`
2. `Meaning`
3. `Pronunciation`
4. `Note`

Card templates render `Front` on the front side, and `Meaning`, `Pronunciation`, and `Note` on the back side. Target decks (e.g. `German`, `Hungarian`) are created automatically on import.

## Instructions for the agent

1. Read `README.md`. If `Vocab/anki-export.txt` exists, read it so you do not create cards for words the user already has.
2. Build the list of words to add. Include EVERY new word or expression from the source:
   - **Full Vocabulary Capture:** Every single new word that appears anywhere in the source (explanations, phonetic examples, sample sentences, drills, checks, or vocabulary lists) that the user does not already have in their known vocabulary / `Vocab/anki-export.txt` MUST be turned into a card. Never omit, sample, or orphan new words.
   - **`Front`:** The target-language word or phrase.
     - **Nouns:** include article/gender marker plus singular, and plural form (e.g. `der Tisch, die Tische`).
     - **Verbs:** infinitive form.
     - Useful chunks and fixed expressions are allowed.
   - **`Meaning`:** Clear and concise English translation.
   - **`Pronunciation`:** Phonetic breakdown, stress guide, or transcript (e.g. `TISH`, `AHL-mosh`). Can be left empty if obvious.
   - **`Note`:** Part of speech, grammar hints, irregular forms, root words, or short usage context (e.g. `Noun (m)`, `Verb (irregular: schläft, schlief)`, `Adjective (álom = dream)`). Leave empty if not needed.
3. Write one file per source: `Vocab/Anki_Imports/<source-id>-cards.txt`.
4. Format: UTF-8, **tab-separated**, one note per line, no tabs or line breaks inside fields. Start with these header lines:

```
#separator:tab
#html:true
#notetype:Language Vocab
#deck:<Target Deck, e.g. German or Hungarian>
#tags column:5
```

   Then the data lines: `Front<TAB>Meaning<TAB>Pronunciation<TAB>Note<TAB>Tags`
5. Tags: space-separated, always include the source ID (e.g. `L004`) and the phase (`familiarization` or `loop`).
6. Examples:

   German:
   ```
   der Tisch, die Tische	table	TISH	Noun (m)	L004 familiarization
   ```

7. Show the file content in chat, and include the file in the task's change list (README §9). Remind the user: import with File → Import in Anki. Re-importing the same file does not duplicate notes, because the first field (`Front`) is used to detect duplicates.

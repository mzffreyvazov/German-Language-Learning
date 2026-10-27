# create_anki_cards: card import format (shared spec)

**Purpose:** define exactly how vocabulary import files are produced. Other prompts (`learn_lesson`, `generate_reading`, `create_test`, `gate_test`) call this. It can also be run on its own.

## User inputs (only when run standalone)

- Words or phrases to turn into cards (or the source ID, e.g. L004): {{source}}

## One-time Anki setup (user)

Create a note type named **`Lang Vocab`** with these fields, in this order:

1. `Word`
2. `Translation`
3. `Example`
4. `ExampleTranslation`

Add the card templates you want (recognition: Word → Translation; production: Translation → Word is optional). Create the target deck once (e.g. `German`).

## Instructions for the agent

1. Read `README.md`. If `Vocab/anki-export.txt` exists, read it so you do not create cards for words the user already has.
2. Build the list of words to add. Only new words from the source, with these limits and rules:
   - Max 15 cards per lesson or reading. If more are needed, say so and let the user choose.
   - **Nouns:** article (or gender marker) plus singular, and plural in the `Word` field (e.g. `der Tisch, die Tische`).
   - **Verbs:** infinitive; put irregular forms inside the example sentence.
   - Phrases are fine as a `Word`.
   - The example sentence must be natural, short, and use mostly words the user already knows.
3. Write one file per source: `Vocab/Anki_Imports/<source-id>-cards.txt`.
4. Format: UTF-8, **tab-separated**, one note per line, no tabs or line breaks inside fields. Start with these header lines (verify they work with the user's Anki version; recent versions support them):

```
#separator:tab
#html:false
#notetype:Lang Vocab
#deck:German
#tags column:5
```

   Then the data lines: `Word<TAB>Translation<TAB>Example<TAB>ExampleTranslation<TAB>Tags`
5. Tags: space-separated, always include the source ID (e.g. `L004`) and the phase (`familiarization` or `loop`).
6. Example (German):

```
der Tisch, die Tische	table	Der Tisch ist groß.	The table is big.	L004 familiarization
```

7. Show the file content in chat, and include the file in the task's change list (README §9). Remind the user: import with File → Import in Anki. Re-importing the same file does not duplicate notes, because the first field is used to detect duplicates.
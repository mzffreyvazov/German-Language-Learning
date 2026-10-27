# extend_roadmap

**Purpose:** propose the next lessons so the roadmap never runs out, using actual progress.
**When:** when 2 or fewer unchecked lessons remain, or whenever the user wants to change direction.

## User inputs

- Wishes or priorities (optional, e.g. "I need to be able to order food soon"): {{notes}}

## Instructions for the agent

1. Read `README.md`, `00-State.md`, `01-Roadmap.md`, `Mistakes.md`, the last few lesson notes (checks and mastery), and recent readings and tests.
2. Decide how many entries to add so that the detailed section has up to 5 unchecked entries (README §7). Never plan further than that in detail.
3. Choose lessons by priority:
   a. Remedial or review lessons for `shaky` lessons and for mistake patterns with count ≥ 3 and status `open` (set the pattern's `linked_lesson` and status `lesson-planned` when you propose it).
   b. The next logical step in grammar and topics, ordered by how much each blocks reading ability in the target language.
   c. The user's stated wishes.
4. Place a gate entry when appropriate (README §7), such as the writing-unlock gate once writing readiness looks plausible.
5. For each proposed entry give: ID (from Counters), name, type, a 2-4 sentence description stating scope explicitly (defining full boundaries for closed sets, or noting how a large closed set is divided), and one line on why it is next. Do not write lesson content.
6. Update the coarse map if the plan changed. Keep it names only.
7. Follow README §9: change list (Roadmap, State next lessons and counters, Mistakes status changes), confirm, write, verify.

# end_session

**Purpose:** wrap up a session: log it, and make sure no updates are left pending.
**When:** at the end of any session. Especially if the session ended without a task prompt finishing.

## User inputs

- Anything you want recorded (optional): {{notes}}

## Instructions for the agent

1. Read `README.md` and `00-State.md`. Review the conversation: what was done, which IDs were touched.
2. Check for **pending updates**: lessons studied or checked but not recorded, mastery changes, mistake patterns noticed, Anki cards promised but not written, roadmap statuses out of date. List them as part of the change list. Never guess at scores: ask if you are unsure.
3. Write the session summary: 3-6 lines of what was covered, plus one line `next:` stating what the user should do next (a specific prompt or lesson ID).
4. Propose the change list per README §9: new `Logs/YYYY-MM-DD-N.md` (README §6.7), State `Last session` section, plus any pending updates.
5. Confirm, write, verify against the schemas.
6. Tell the user the summary can be pasted into the next session's prompt if they are in chat-only mode.
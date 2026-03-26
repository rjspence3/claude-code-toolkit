---
description: "Save context and prepare a clean handoff for a new session. Writes a handoff doc, pushes, and gives you the exact prompt to paste."
---

Prepare to hand off to a fresh session:

1. Write a handoff document using the /handoff format. Save it to the current directory.
2. Tell me the filename.
3. Run `git push` in the current working directory. Report success or failure.
4. Output the exact prompt I should paste into the new session as a copyable code block. The prompt should reference the handoff file path and nothing else — the new session will read the file for full context.

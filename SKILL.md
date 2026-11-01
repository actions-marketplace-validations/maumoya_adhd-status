---
name: adhd-status
description: Zero-reading status board for ADHD brains. Use whenever the user says STATUS (any case, alone or inside a sentence), "status?", "where are we", "what's left", "what do I do now", or "lazy adhd rule" during a working session. Silently sweeps the whole conversation so nothing is forgotten, then replies with one tiny Task | Why | Status table and exactly one next action the user can click or run. Checks results itself instead of asking the user to paste output.
license: MIT
---

# adhd-status

The user has ADHD. Long replies get skipped, open questions get lost, and "tell me when it's done"
means it never gets done. When they say **STATUS**, give them one tiny table and one next step.
Nothing else. The value is that they can glance, click, and move on.

## Step 1: sweep the whole conversation (silently)

Before writing anything, walk the **entire** conversation, not just the last few messages. List
every item that was started or promised:

- tasks, sub-tasks and side jobs (background runs, other sessions, subagents)
- changes waiting to be reviewed, merged, deployed or released
- commands you handed the user to run
- promises you made ("I'll check after…", "next I'll…")
- questions you asked that never got an answer

Mark each one: **done**, **running**, **waiting on the user**, **waiting on something else**, or
**forgotten**. Forgotten items are the reason this skill exists: surface them as rows, never drop
one quietly.

If a completion-discipline skill such as `unlazy` is installed, run its gate check for this sweep.
If not, the checklist above is the sweep.

Do the sweep with your own reasoning and tool calls only. Do not print the list, the gates or any
notes about the sweep; the user only sees the result.

## Step 2: check, don't ask

The user should never have to report back to you.

- For any command you gave them, look at the result yourself: read their terminal if your host can
  (a terminal-reading tool), otherwise check the effect (the file exists, the service answers, the
  log line is there). Only if nothing can be checked, ask with a button ("Ran it" / "Not yet"),
  never with "paste the output".
- Verify live state with one cheap call each when it matters (CI status, a job's last line, a
  session's last report). Read only the lines you need. Do not re-read what is already in context;
  that wastes tokens.

## Step 3: the table

```
| Task | Why | Status |
|---|---|---|
| Login page | users can sign in | 👉 you: run test |
| Nightly backup | don't lose data | 🔄 running |
| Old API cleanup | forgot it earlier | ⚠️ not started |
```

- At most 4 rows. Needs-the-user and problems first, then running, then done.
- Done items only if they finished since the last STATUS; otherwise they are noise.
- More than 4 open items? Keep the 3 most urgent and make row 4 `+N more | parked | ⏸`.
- **Task**: 2–4 plain words. No links, ticket or PR numbers, branch names, hashes or file paths.
- **Why**: up to 6 plain words, the benefit to the user, not the mechanism.
- **Status**: one emoji and up to 5 words.

| Emoji | Means |
|---|---|
| ✅ | done / live |
| 🔄 | running, nothing for you to do |
| 👉 | your turn (it is the next action below) |
| ⏸ | waiting on something else |
| ⚠️ | problem or forgotten |
| ❌ | failed |

### Context check (compacting)

Very long conversations get slow, expensive and forgetful. If this conversation is long (many
tasks done, big tool outputs, or your host reports high context use) and nothing urgent is open,
the next action may be to compact or start fresh: offer it as a row (`Long thread | faster, cheaper
replies | 👉 compact`) and as the next action, using the host's command (for example `/compact` in
Claude Code). Before suggesting it, make sure every open item is in the table, so nothing is lost.

## Step 4: one next action (only if the user must act)

Exactly one, directly under the table:

- **A decision**: clickable option buttons if your host has them (a question tool or next-step
  buttons): recommended option first, at most 2 options. Without buttons, a numbered pick list of
  at most 2 the user can answer with one key ("1" or "2").
- **A command**: one fenced `bash` block, one command, no `$` prompt, no output inside, so it can be
  run or copied in one click. Next turn, check its result yourself (Step 2) without being asked.
- **A click somewhere**: one line saying exactly where to click, plus a "Done" button.

If nothing needs the user, give no button. The last row's status can say "nothing for you".

## Example

User: `status`

Bad (makes them read and type):

> Great question! So far we refactored the auth module, and I opened a pull request. The CI is
> running now. Once it passes, you might want to merge it. Also, earlier I mentioned the backup
> script; could you run it and paste the output so I can check it? Let me know how you want to
> proceed!

Good:

| Task | Why | Status |
|---|---|---|
| Backup script | don't lose data | 👉 you: run it |
| Auth cleanup | fewer login bugs | 🔄 tests running |

```bash
./scripts/backup.sh
```

## Never

- Prose before the table, a paragraph after it, or a recap of how you got here.
- A list of future steps. One step per STATUS.
- A question the user must type an answer to when a button or one key would do.
- Links in the table (a link may go in the action line only if the user must open it).
- Asking the user to paste output or "let me know when done".

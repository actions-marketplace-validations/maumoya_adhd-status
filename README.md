# adhd-status

A skill for Claude Code (and other agents that read `SKILL.md` skills) that turns **STATUS** into
a zero-reading status board.

Say `STATUS` in any working session and you get:

- one tiny **Task | Why | Status** table (4 rows max, plain words, emoji status)
- exactly **one** next action you can click or run
- nothing forgotten: the agent silently sweeps the whole conversation first, so promised-but-dropped
  work shows up as a row
- no "paste the output" or "tell me when done": the agent checks results itself
- a nudge to compact or start fresh when the conversation gets long and slow

Built for ADHD brains, useful for anyone juggling several things in one session.

See [`examples/status-reply.md`](examples/status-reply.md) for a sample reply.

## Install

Claude Code loads personal skills from `~/.claude/skills/`.

Copy:

```bash
git clone https://github.com/maumoya/adhd-status.git
mkdir -p ~/.claude/skills/adhd-status
cp adhd-status/SKILL.md ~/.claude/skills/adhd-status/
```

Or symlink, so `git pull` updates it:

```bash
git clone https://github.com/maumoya/adhd-status.git ~/src/adhd-status
ln -s ~/src/adhd-status ~/.claude/skills/adhd-status
```

For one project only, put it in that project's `.claude/skills/adhd-status/` instead.

## Use

Type any of these in a session:

```
STATUS
status?
where are we
what's left
```

## Optional extras

- **`unlazy`** or another completion-discipline skill: if installed, the sweep uses its gate check.
  Without it, the skill uses its own built-in checklist.
- **Clickable buttons / terminal reading**: if your app offers option buttons or a tool that reads
  your terminal, the skill uses them. Otherwise it falls back to a one-key pick list and checking
  results directly.

## Test it (benchmark)

The `evals/` folder holds test cases in the format `claude plugin eval` reads. From this folder:

```bash
claude plugin eval .
```

It runs each case with and without the skill and reports the score difference. These are real
model runs and are billed to your account.

## License

MIT. Copyright (c) 2026 Mauricio Alonso Moya Tapia.

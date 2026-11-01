# Privacy and safety

adhd-status is **plain text**: Markdown instructions, test cases and images. You can check every
claim below yourself; the whole repository is a few dozen readable files.

## What this project does not do

- **No code runs.** There are no scripts, binaries, installers or dependencies in this repository.
- **No tracking.** No analytics, telemetry, pixels or phone-home of any kind.
- **No data collected.** The author receives nothing from you: no conversations, files, usage or
  contact details.
- **No network calls.** The skill never tells your agent to send anything to any server.
- **No backdoors.** No hidden instructions; `skills/adhd-status/SKILL.md` is the entire skill.

## What the skill does ask your agent to do

Inside your own session, the skill tells your agent to read things it can already see: the
conversation, your terminal output, CI status and your project's `ROADMAP.md`. That stays between
you and your AI tool. How your AI tool handles your data is set by that tool's own privacy policy
(for Claude Code, Anthropic's), not by this project.

## Check it yourself

```bash
git clone https://github.com/maumoya/adhd-status.git && cd adhd-status && git ls-files
```

Every file is Markdown, YAML, JSON, plain text or an image.

## Reporting a problem

Open an issue at https://github.com/maumoya/adhd-status/issues.

# software-factory (placeholder name)

How I run coding agents day to day. Two models split the work. Claude writes the code and makes the design calls. Astra (a Codex model) reviews, debugs, and checks things visually. Everything here is a snapshot of my live config, copied as-is.

## What's here

- `AGENTS.md` is the shared rulebook every agent reads, in Claude Code and Codex alike. It sets up the vocabulary (two-way door, ground truth, drive-by, hearsay) and the rules that hang off it. On my machine it lives at `~/.agents/dara-way.md`, and `~/.codex/AGENTS.md` is a symlink to it.
- `CLAUDE.md` is my global Claude Code file. It imports `AGENTS.md` and adds who does what between Claude and Astra.
- `skills/` holds the skills that go with it:
  - `dara-pr-making-rules`: how a change goes from repro to merged PR, including what evidence each kind of change needs.
  - `windows-verification` (Claude side) hands a "can you test this PR on Windows" request to Astra, which runs `windows-pr-review` (Codex side).
  - `unslop`: strips AI tells from writing. Applied to everything, PR descriptions included.
  - `writing-for-agents`: how to write skills and AGENTS.md files.
  - `grilling`: question rounds to stress-test a plan.
  - `domain-modeling`: keeps a project's CONTEXT.md glossary and ADRs current.
  - `mock`: side-by-side UI variants on one static page, each changing one thing.
  - `blueprint-docs`: house style for standalone HTML reports and plans.

The `agents/openai.yaml` files are Codex metadata. Claude Code ignores them.

## Install

Copy `AGENTS.md` and `CLAUDE.md` into `~/.claude/` (or `~/.codex/AGENTS.md` for Codex), and copy the skill folders into `~/.claude/skills/` or `~/.codex/skills/`. Some details are mine: the drive layout in AGENTS.md, the T3 Code `delegate_task` calls, the `gpt-6-astra` model name, and an R2 upload skill that isn't in this repo. Swap in your own.

## Credits

`writing-for-agents`, `grilling`, and `domain-modeling` come from [mattpocock/skills](https://github.com/mattpocock/skills) (MIT, license in `skills/THIRD_PARTY_LICENSE_mattpocock.txt`). `writing-for-agents` is unchanged; the other two have small edits.

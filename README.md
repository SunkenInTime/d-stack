# d-stack

How I run coding agents day to day. Two models split the work. Claude writes the code and makes the design calls. Astra (a Codex model) reviews, debugs, and checks things visually. Everything here is a snapshot of my live config, copied as-is.

## What's here

- `AGENTS.md` is the shared rulebook every agent reads, in Claude Code and Codex alike. It sets up the vocabulary (two-way door, ground truth, drive-by, hearsay) and the rules that hang off it. On my machine it lives at `~/.agents/dara-way.md`, and `~/.codex/AGENTS.md` is a symlink to it.
- `CLAUDE.md` is my global Claude Code file. It imports `AGENTS.md` and adds who does what between Claude and Astra.
- `skills/` holds the skills I use with it:
  - `unslop`: strips AI tells from writing. Applied to everything, PR descriptions included.
  - `writing-for-agents`: how to write skills and AGENTS.md files.
  - `mock`: side-by-side UI variants on one static page, each changing one thing.
  - `dara-pr-making-rules`: how a change becomes a PR, from first repro to merged: ground truth by what the user meets, a before/after lede, Astra review, then seeing CI through. Its attachment guide uploads through an `upload-to-r2` skill that isn't included here; swap in your own file host.
  - `astra-loop`: settles a contested design or decision by going rounds with a second model until they converge.

The `agents/openai.yaml` files are Codex metadata. Claude Code ignores them.

## Install

Copy `AGENTS.md` and `CLAUDE.md` into `~/.claude/` (or `~/.codex/AGENTS.md` for Codex), and copy the skill folders into `~/.claude/skills/` or `~/.codex/skills/`. Some details are mine: the drive layout in AGENTS.md, the T3 Code `delegate_task` calls, and the `gpt-6-astra` model name. Swap in your own.

## Credits

`writing-for-agents` is unchanged from [mattpocock/skills](https://github.com/mattpocock/skills) (MIT, license in `skills/THIRD_PARTY_LICENSE_mattpocock.txt`).

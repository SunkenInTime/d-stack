---
name: dara-pr-making-rules
description: How Dara's PRs get made, from first repro to merged. Use when a fix or change will become a PR, when opening or updating a PR, writing its description, or babysitting CI and review bots.
---

A PR is done when I'd let you merge it without reading the diff. The words below build on the ones in the global file (ground truth, raison d'être, drive-by, verschlimmbessern, hearsay).

## Leading words

repro - the bug happening in front of you, on the surface where users hit it. not a theory of the bug.

chesterton's fence - behavior that looks wrong but was put there on purpose. find out why before you remove it.

lede - the one image, GIF or number that shows the change. don't bury it.

noise - a review comment that's wrong or doesn't matter. it gets a reason, not a code change.

tautology - a test that can't fail. it restates the code, checks a mock returns what you told it to, or checks a config exists.

## Repro first

Get a repro before you edit. If it won't happen, force it: synthesize the trigger or instrument until it fires. In a repo we don't own, check for prior work and chesterton's fences first: open and closed PRs, the issue's triage, tests that pin the current behavior. Bug fixes can go. Features and default changes need the maintainers' direction first, so draft that ask instead of the code. If a maintainer closed an earlier PR, open a fresh one that links it.

## Root, not symptom

Smallest diff that fixes the cause. A fix that grows every review round is treating symptoms; that's verschlimmbessern, step back and find the layer where the problem lives.

## Ground truth by what the user meets

Before each push, run the checks covering what you touched; CI confirms the whole.

Tests earn their place. Before writing one, ask what it would catch that nothing else would. Keep the ones that fail without the fix and pass with it, or that guard something users would feel if it broke. No tautologies. Plenty of changes need no new test; a visual or timing change is usually proven by ground truth, not a test.

Then classify the change by what the user meets, not where the code lives. A server change that alters a label, icon, color or timing on screen is visual.

- visual: before and after from a real running client, same build, only the fix toggled. Crop when the change is small.
- interaction or timing: also a GIF, 8 to 15 s, trimmed to the moment, readable at ~1000 px. GitHub won't play a linked MP4.
- backend with no visible effect: a live run of the real flow, its output, how long it took.
- more than one person involved: two real accounts on the sign-in providers users actually use.

A unit test alone isn't ground truth for a bug someone saw at runtime. Capture and upload: [Attachments](references/uploading-artifacts.md).

## Second eyes

Beyond a small fix, have Astra review through T3's `delegate_task`. Ask for blockers first (breaks or loses work under normal use), then tautologies, then style. Its findings are hearsay until you've looked: fix, consider or dismiss each. Three rounds finding the same family of bug means the design is wrong.

## The description

Readers skim. Lede first. Then the raison d'être and what existed before, what this PR does and why this approach, how far into ground truth it got, and what isn't covered. Core logic before wiring; mechanical changes as a plain list. No `## Summary` / `## Test plan` boilerplate on small PRs. Fit this into the repo's template where there is one. unslop it before posting. In a repo we don't own, also draft the message I'd send the maintainer: lowercase, a few lines, the link.

## See it through

Every push restarts CI, so batch fixes into one push per round. Watch checks in the background (`gh pr checks <PR> --watch --fail-fast`; restart it if it sees no checks right after a push) and keep working meanwhile. Bots catch real bugs and also file noise; a stuck or rate-limited bot doesn't get to hold the PR hostage. Where the repo's AGENTS.md says agents merge, merge once checks pass, the review is clean and you've reached ground truth, and recheck that it still merges after a sibling lands. Otherwise leave it for the maintainers.

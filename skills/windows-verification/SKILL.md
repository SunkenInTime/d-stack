---
name: windows-verification
description: Run a PR test on Windows for someone who asked Dara, by handing it to Astra's windows-pr-review skill. Use when a contributor asks Dara to test or check their PR on Windows (optionally Linux too).
---

# Windows verification (delegated)

Someone sends Dara a PR and asks for a Windows test, often with one feature to check ("make sure snapshots still works"). Astra runs the whole review through Codex's own `$windows-pr-review` skill at `C:\Users\shawn\.codex\skills\windows-pr-review\`. That skill owns the process: pinned worktree, checks, computer use on Dara's desktop, Blueprint report, GitHub verdict, review record. Read its `SKILL.md` and `references/windows-verification.md` before dispatching so you know what a complete run produces and can audit it.

## Process

1. Resolve the request.
   Get the PR URL, the asker, and any feature they named, from the message or screenshot. Check the PR is open and note its head SHA. Look for a review record from an earlier run of the same PR.
   Completion: PR, head SHA, named features, and whether this is a follow-up are known.

2. Settle publishing.
   The GitHub review is posted as Dara. Post only when Dara has said to in this request; otherwise Astra stops with the report and exact review text ready.
   Completion: you know whether Astra may publish the report and post the review.

3. Dispatch to Astra with T3's `delegate_task` (`gpt-6-astra`, reasoning `high`; provider instance from `orchestrator_capabilities`):

   > Use $windows-pr-review on <PR URL>. <Asker> asked Dara to test it on Windows and specifically check: <named features>. Publishing: <authorized to upload the report and post the review / stop with the report and review text ready, do not post>. Do not merge or push. Touch only the app instances you launch. Finish with the verdict, the observed result of each named feature with screenshot paths, report and review links or drafts, and the worktree path.

   Completion: the task finishes with a verdict report.

4. Audit the evidence.
   Open the screenshots. Each named feature needs a visible observed result on the tested SHA. Check that tool failures are labeled apart from app defects and that every finding has a reproduction.
   Completion: every named feature is seen working, seen broken, or its gap is named.

5. Linux, when asked.
   Run it in WSL Ubuntu yourself, following the `wsl-posix-verification` memory (clone on ext4, Dara approves the C: exception each time). Test the same named features. If WSL can't get there, say what blocked it.

6. Report to Dara: verdict, each named feature's result, Linux result if run, the report link, and the posted review or the draft text to send.

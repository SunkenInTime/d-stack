---
name: windows-pr-review
description: Review Windows PRs in isolated worktrees with computer-use testing, Blueprint reports, GitHub verdicts, and follow-up reviews.
---

# Windows PR review

User-invoked only. `agents/openai.yaml` disables implicit invocation.

Carry each requested PR from a pinned commit through Windows testing to an evidence-backed verdict. Use the user's existing authorization for publishing reports and GitHub reviews. When publishing is outside that authorization, finish the report and exact review text before asking to post. Invocation alone does not authorize merging or pushing fixes.

## 1. Establish what changed

Resolve PR numbers from the message, links, or attached images. Read repository instructions and fetch GitHub state, draft status, head/base SHAs, prior reviews, and relevant new discussion. Use available GitHub tools or `gh`.

For a follow-up, compare these with the last reviewed SHAs and findings:

- Same head and base: report status/discussion changes. Preserve the prior verdict; skip duplicate tests, reports, and comments unless new evidence challenges a finding.
- New head or materially changed base: inspect the delta and reassess affected behavior, including previous findings.
- Merged or closed: report that state. Review new evidence if relevant, but submit approval only for an open PR.

For stacked PRs, distinguish the incremental diff from inherited changes and also test the resulting branch. Finish this step with a recorded comparison range and a reason for every PR that needs further work.

## 2. Isolate the run

Create or reuse one dedicated worktree per PR, checked out at its exact head. Preserve dirty worktrees by choosing a fresh path. Keep evidence outside source control. Discover scripts, package-manager versions, and native build requirements from the checkout.

Use separate state directories, ports, and fixtures for each app instance. Record launched process IDs and command lines for cleanup. Avoid importing the user's real app data during onboarding. Complete setup when each worktree's SHA, dependency setup, test commands, and isolated app configuration are known.

## 3. Review and reproduce

Read [Windows verification](references/windows-verification.md) before native or UI testing. Read the available computer-use skill before operating Windows apps, and the browser-control skill when using an existing browser session. Resolve those skills from the current catalog; tool names and plugin paths change.

Inspect each changed behavior and its callers, protocol boundaries, failure handling, and tests. Run repository-required format, lint, and type checks, relevant focused tests, and native formatting/tests/builds where affected. For T3 Code, honor the user's required `bun fmt`, `bun lint`, and `bun typecheck`; use `bun run test`, never `bun test`.

Use computer use for the affected user flow on the actual Windows app. Supplement it with native or public RPC probes where they provide stronger evidence. A direct RPC test is not a frontend acceptance test. Follow lifecycle tests through cleanup and inspect asynchronous errors after acknowledgements.

For a suspected defect, produce the smallest useful reproduction or regression test and record expected versus observed behavior. Compare the relevant path and dependency with the base to establish whether the change introduced it. Classify baseline bugs and optional scope cleanup separately. Keep review-only fixtures out of the production diff.

Testing is complete when each affected behavior has evidence or an explicit limitation, mandatory checks have recorded outcomes, and every blocking finding has a reproducible trigger and source location. A failed required check remains a failure; distinguish an environmental blocker from a code defect.

## 4. Build the review artifact

Use the available blueprint-docs skill to create one standalone HTML review/design report per reviewed PR. Include:

- PR number, pinned SHAs, date, environment, verdict, and comparison scope.
- Numbered findings with severity, source links, reproduction, expected/actual result, and recommended correction. Label baseline issues and optional cleanup.
- A concise explanation or diagram of the changed design.
- Verification results, UI evidence, and untested boundaries.
- Reproducible commands and relevant logs or an evidence bundle.

Use upload-to-r2 to publish the report and selected evidence when sharing is authorized. Build bundles from an explicit allowlist; keep credentials, private app state, unrelated browser content, and token-bearing server logs local. Inspect screenshots before including them.

Verify the HTML in a browser at desktop and narrow widths, in both themes. Exercise diagrams/tooltips and artifact links when present. Finish with readable reports and working download URLs, or a clearly recorded hosting limitation.

## 5. Publish the verdict

Re-fetch head/state immediately before posting. If the head moved, review the new delta and update evidence before approving. Pin the submitted review to the tested commit.

Post the numbered findings and report/evidence links. Request changes for reproducible in-scope defects; approve when the scoped change passes review with sufficient validation. Use a comment when validation is incomplete or a formal review is unavailable. Explain material test limits even when existing tests pass. Recommend small cleanup without conflating it with blockers.

For re-reviews, state which prior findings are resolved, remain, or are superseded. Link the previous review and revised report. Publish one substantive update per reviewed head; this workflow ends after the requested pass rather than polling indefinitely.

Use structured API arguments or a body file for multiline review text. Verify GitHub returned the expected state, commit, body, and links. If posting is blocked, retain the ready-to-post artifacts and report the actual failure.

## 6. Leave a resumable result

Stop only the processes identified as belonging to this run, checking identity before termination. Retain worktrees and evidence unless removal was requested. Report PR verdicts, review/report links, significant limitations, and worktree locations.

Save a small local review record beside the evidence: repository/PR, tested head/base, worktree, check outcomes, numbered finding status, report URL, GitHub review ID/URL, and remaining blockers. A later invocation starts by comparing this record with GitHub.

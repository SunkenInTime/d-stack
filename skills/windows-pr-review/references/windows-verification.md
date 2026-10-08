# Windows verification

Choose cases from the changed behavior. The terminal/discovery examples below are evidence from the original workflow, not a mandatory suite for unrelated PRs.

## Native and subprocess boundaries

For a sidecar or process-discovery change, exercise the actual Windows binary as well as mocks. Check protocol version alignment, request correlation, timeout/interruption cleanup, and restart behavior where affected. Include missing/unavailable native support when the code has a fallback.

For subprocess fallbacks, distinguish runner failure, nonzero exit status, truncated output, malformed output, and valid empty results. Check what happens to the last successful snapshot and retry cadence. A runner may return a nonzero exit as ordinary data; a parsed empty string alone does not prove successful discovery.

For terminal/process ownership, use an owned fixture and verify ancestry, activity start/stop, ownership attribution, and removal after exit. For port discovery, include IPv4/IPv6 where supported and verify that closing the fixture removes its listener.

For terminal lifecycle changes, exercise open, command execution, interruption, restart, and close, including close-before-ready when relevant. An RPC acknowledgement can precede an asynchronous crash. Capture cleanup outcome separately from earlier successful operations.

Treat POSIX signals and Windows PTY termination as distinct interfaces. If a crash appears on the unchanged adapter or dependency, reproduce it against the base or a minimal dependency example before assigning regression status.

## Computer-use evidence

Open the isolated app through the supported computer-use/browser tools, inspect its current state, and walk the changed flow. Capture the visible result with the PR/worktree identifiable. Use ordinary app controls and keep the scope on the review instance.

If the automation bridge fails, distinguish a transport/emulation error from an application defect. Try an available supported surface and continue independent code/native checks. Record any user flow that remains unverified. A tool failure is not a PR finding without application evidence.

For a terminal/discovery flow, a useful sequence is opening the terminal drawer, running a local HTTP fixture, checking its activity and discovered preview, stopping it, and checking disappearance. Public RPC probes may supplement repeated lifecycle checks; describe exactly which steps were visually tested.

## Performance evidence

Match the claim to the measurement. Request latency and functional success do not establish lower CPU or fan noise. For a CPU regression claim, compare base and head under the same workload, record duration and process ownership, and account for helper-process creation and cleanup. Report sample size and environment; label unmeasured soak, installer, and architecture coverage.

## Evidence and cleanup

Capture exit codes alongside command output. Keep raw private logs separate from shareable artifacts. Store the tested SHA with probes so results survive force-pushes.

Identify fixture processes by recorded PID plus command line or owned port before stopping them. On Windows, use native PowerShell file operations and verify resolved paths before recursive removal or moving. Prefer retaining the worktree over deleting uncertain state.

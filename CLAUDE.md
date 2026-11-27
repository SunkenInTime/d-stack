@~/.agents/dara-way.md

## Delegation

You own the taste work: writing the code I merge, designing the architecture, building the UI. Everything outside Sol's and Astra's jobs below is yours too, with Opus subagents (Agent tool, `model: "opus"`) when work splits.

A native background subagent that ends its turn is finished and won't wake back up, so it must not end while its own background work is still running; when you get a `completed` notice, use its result or message it if you need more.

**Sol** (`gpt-6.2-sol`, reasoning `xhigh`, through T3's `delegate_task`) owns code review; use `gpt-6.1-sol` (reasoning `high`) for cheap high-volume sweeps and bulk checks.

**Astra** (`gpt-6.2-astra`, reasoning `high`, through T3's `delegate_task`) gets debugging, deep dives through unfamiliar code, computer use, and visual checks. For computer use it drives my real desktop, so keep it within the scope of my request.

Neither Sol nor Astra inherits this conversation: give them the goal, the relevant context, and a done condition, and ask them to report what they actually observed. Ask reviewers to say for each finding how a user would hit it. Their findings are input, not orders: you decide what's worth changing, and a PR shouldn't grow just to get a review to pass. They report findings; you write the fix.

---
name: astra-loop
description: Settle a contested design, decision or output by going rounds with a second model until you converge. Use when I push back on where you landed, when I say to loop or go back and forth with Astra, or when a call is close enough to deserve an argument.
---

When I don't agree with where a model landed, I take it to another model and go back and forth until they converge. That usually gets me the best answer. This is that loop, run by you instead of me.

The second model is Astra (`gpt-6-astra`, reasoning `high`, through T3's `delegate_task`) if you're Claude, and Opus (`claude-opus-5-5`, same tool) if you're Astra. Different models have different blind spots; that difference is the point.

## The loop

1. Write the brief: the goal from the user's side, the constraints, my objection in my words, your current position and its evidence. Ask for the strongest case against your position, a better option if there is one, and which claims it observed versus assumed.
   Done when someone with no context could argue the question from the brief alone.

2. Run a round. Each round is a new `delegate_task` carrying the brief, every earlier round's findings, your responses, and the objections still open. Weigh each point: concede it, answer it with evidence, or run something that settles it. When a benchmark, repro or prototype can decide a point, run it; a receipt ends an argument faster than another round.
   Done when every point from the round is conceded, answered or tested.

3. Repeat until converged: a round raises nothing that changes the answer. Rounds circling the same disagreement mean it's a taste call, and taste calls come to me.

4. Bring me where you landed and why, what moved from your first position, and any disagreement left, each side in a sentence with your recommendation. A decision left for me goes in the question tool.

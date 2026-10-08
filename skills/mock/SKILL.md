---
name: mock
description: Mock variants of an existing UI on one static comparison page, each a faithful reproduction of the real UI with one variable changed. Use when the user asks for mocks or variants of a screen or component.
---

Every page is built from [`harness.html`](harness.html); the comment at its top is the template contract. Baseline is the control: mock 1 reproduces the current UI, and every other mock is Baseline with one variable changed, so the user reads the difference and nothing else.

1. **Ground truth.** Find the code that renders the UI and every visual property it resolves to: tokens (CSS custom properties, Tailwind theme, font stack and sizes, radii, shadows, spacing), markup structure, real copy, icons (copy the SVG paths), and the dark mode mechanism. If the app runs, screenshot the real screen (the `run` skill or the preview tools) as the reference. Done when every visual property has a source file you can name.
2. **Baseline.** Tokens and base styles go in `shared-head` so every mock inherits them, with the font the app loads. Build mock 1 from the real markup and compare it against the screenshot until the remaining difference is one you would accept in a screenshot diff. For a screen that does not exist yet, Baseline is the surrounding app chrome with the new area empty. Variants start only from a matched Baseline.
3. **Variants.** Duplicate the Baseline markup and change one variable per mock, named for the idea, with a one-sentence `data-note` on what changed and why. Data reads as real: plausible names, dates, and amounts. Usually three to six mocks, or the number the user asked for.
4. **Interaction.** The states the decision hinges on work: hover, open and close, tabs, selection, validation, and empty or loading states where they matter. Each mock's script lives in its own template.
5. **Show it.** Copy the harness to `<project>/.mocks/<slug>.html`, fill in the title, shared head, and mock templates, then `open` the file. Done when every mock has been viewed in the preview tools at the widths that matter with nothing clipped, and each interaction from step 4 has been exercised once. Report the path and one line per mock.
6. **Iterate.** Follow-ups edit the same file: add, drop, or revise variants; Baseline stays. Applying the chosen variant to the real code is separate work, done when the user picks.

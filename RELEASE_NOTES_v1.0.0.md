# v1.0.0 — First public release

The `interactive-educator` skill, in a state ready for general use.

## What it does

Adds a `/aha <topic>` command to Claude. After three or four quick MCQs about your background, the anchor domain, and your visual taste, Claude produces a 4-screen interactive React artifact that teaches the topic by guided discovery. Inspired by 3Blue1Brown's pedagogical style.

## Installation

1. Download `interactive-educator.skill` (attached below).
2. In Claude: **Settings → Capabilities → Skills → Upload skill**.
3. Type `/aha gradient descent` (or any topic) to use.

Requires a Claude plan with skills support (Pro, Max, Team, Enterprise).

## Design choices in this release

- **Trigger:** `/aha <topic>`, with `/3b1b <topic>` as an alias.
- **Phase 1:** 3 mandatory MCQs (prior knowledge, anchor domain, theme), plus a 4th conditional question about mental model that only fires for non-beginners.
- **Phase 2:** Exactly 4 screens — Puzzle → Explore → Name → Challenge.
- **No quizzes inside artifacts.** Verification happens through the visualization itself.
- **Anchor domain shapes scenarios, not just labels.** Example: choosing "courtroom" for Bayes produces evidence/verdict scenarios, not the same artifact with renamed variables.

## What's known to work well

- Math concepts with clear geometric structure (eigenvectors, gradients, derivatives, Fourier)
- ML concepts grounded in optimization or probability (gradient descent, Bayes, attention)
- Probability / statistics where simulation reveals the answer

## What's known to be harder

- Highly algebraic concepts with no natural visual (some abstract algebra)
- Topics that require the learner to already hold three or four concepts in mind simultaneously — these may be better split into a sequence of `/aha` calls.

## Files in this release

- `interactive-educator.skill` — the installable skill bundle.
- Source files (`SKILL.md`, `references/build-spec.md`) and an example artifact (`examples/gradient_descent.jsx`) live in the repository.

## Feedback

If the skill misfires on a topic, file an issue with the specific failure mode. See [CONTRIBUTING.md](../CONTRIBUTING.md) for what's most useful.

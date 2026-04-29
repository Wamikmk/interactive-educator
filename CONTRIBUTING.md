# Contributing

Thanks for considering a contribution. This is a small skill with a strong design philosophy, so a quick orientation will save us both time.

## What kinds of contributions are most useful

In rough order of value:

1. **Bug reports with specific failures.** "I ran `/aha [topic]` and the artifact did [thing]; here's the chat link or screenshot." These are gold. General "the skill could be better at X" feedback is harder to act on.
2. **New worked examples.** If you've used the skill and got a really good output, contribute a markdown writeup to `examples/` (one file per example, e.g. `examples/fourierTransform.md`) with the topic, profile inputs, the aha, screen-by-screen notes, and a Claude.ai share link to the live artifact.
3. **Edits to `build-spec.md`** that fix specific React bugs or environment constraints. The skill encodes hard-won lessons; if you've found a new failure mode, document it.
4. **Trigger improvements.** If `/aha` is misfiring (triggering when it shouldn't, or not triggering when it should), tell me where and how.

## What's intentionally fixed (don't try to change without strong evidence)

A few design decisions are load-bearing. PRs that touch these will get pushback unless they come with concrete evidence the current design is failing:

- **The trigger is `/aha`.** Short, semantically precise, low false-trigger rate. Don't propose `/explain`, `/teach`, `/tutorial` — they all collide with normal conversation.
- **Exactly 4 screens.** The Puzzle → Explore → Name → Challenge structure is mandatory. Earlier versions had "3-5 screens" and produced inconsistent artifacts.
- **Phase 1 has 3-4 questions, not more.** Each additional question costs friction. Cuts to the question set are welcome; additions need justification.
- **No quizzes inside the artifact.** Verification happens through the visualization. This is non-negotiable — quizzes are the failure mode the skill is built to avoid.

## How to test changes

Skills are validated and packaged with the [skill-creator](https://github.com/anthropics/skills/tree/main/examples/skill-creator) tooling.

Quick validation:
```bash
python -m scripts.quick_validate path/to/skill-folder
```

Repackage after edits:
```bash
python -m scripts.package_skill path/to/skill-folder ./dist
```

Manually test: install the new `.skill` file in Claude, run `/aha` on at least three topics covering different domains (math, ML, probability ideally), and confirm the output respects the build spec.

## PR checklist

- [ ] `SKILL.md` description fits Claude's metadata budget (no angle brackets, under ~1000 chars)
- [ ] `build-spec.md` edits don't contradict environment constraints (single file, no browser storage, Tailwind core only)
- [ ] If you changed Phase 1 questions, you've actually run the skill and confirmed the new flow doesn't feel rigid
- [ ] If you changed the screen structure, you've run it on at least 2 topics
- [ ] New `examples/` files are markdown writeups (one `<topic>.md` per example) with topic, profile inputs, the aha, and a live Claude.ai share link

## Issue templates

When filing a "skill produced bad output" issue, please include:

- The exact `/aha [topic]` command you ran
- Phase 1 answers you gave
- What the artifact actually did wrong (be specific — "screen 2 had no interaction," "the formula on screen 3 didn't match the slider," etc.)
- The JSX output or a chat link if possible

Vague issues get closed; specific ones get fixed.

## Style

Match the existing prose in `SKILL.md` and `build-spec.md`: direct, no hedging, examples over abstractions. If you're tempted to write "ALWAYS" or "NEVER" in caps, that's usually a sign the rule isn't carrying its own weight — explain *why* instead.

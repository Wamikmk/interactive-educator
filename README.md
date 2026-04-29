# interactive-educator

> A Claude skill for building 3Blue1Brown-style interactive lessons on demand.

Type `/aha gradient descent` in Claude. Answer three quick questions about your background, the domain you'd like the lesson grounded in, and your visual taste. Get a 4-screen interactive React artifact that teaches by guided discovery not by lecturing.

## Why this exists

Most "explain X" responses from LLMs are summaries accurate, dense, forgettable. The good ones are essays. None of them are *interactive*.

3Blue1Brown's videos work because they engineer a single moment where the viewer goes from confused to "oh *that's* what's actually happening." This skill is an attempt to systematize that for any topic, on demand, in a format you can actually click on.

The trade-off: you give up the elegance of a hand-tuned video. You gain the ability to build one for *your* misconception about *your* topic in a couple of minutes.

## Install

**Requires:** A Claude plan with skills support (Pro, Max, Team, or Enterprise).

1. Download [`interactive-educator.skill`](./interactive-educator.skill) (or grab it from [Releases](../../releases)).
2. In Claude, go to **Settings → Capabilities → Skills → Upload skill**.
3. Select the `.skill` file. That's it.

If you'd rather inspect before installing, the unpacked skill lives in [`skill/`](./skill).

## Use

```
/aha gradient descent
/aha fourier transform
/aha bayes theorem in a courtroom context
/3b1b determinants
```

The skill runs in two phases:

**Phase 1  Profiling.** Three or four multiple-choice questions about your prior knowledge, the domain you want the lesson anchored in, and your preferred visual aesthetic. Total time: ~20 seconds.

**Phase 2  Building.** Claude writes a single React JSX file with four screens: **Puzzle → Explore → Name → Challenge**. The artifact renders inline. You manipulate it. The "aha"  if the build went well happens between screens 2 and 3.

If you want to skip profiling, give Claude full context up front: *"/aha eigenvectors -> I know matrix multiplication, anchor it in image compression, blueprint aesthetic."* The skill will skip questions it already has answers for.

## What you get

Every artifact follows the same structure:

| Screen | Job |
|---|---|
| **1. Puzzle** | Open with a visual mystery in your chosen domain. No definitions. You form a hypothesis by manipulating the visual. |
| **2. Explore** | Discover the underlying pattern through guided interaction. Socratic questions direct your attention; the visual reveals the answer. |
| **3. Name** | Now the formal definition or equation appears and every symbol maps back to something you already moved on screen. |
| **4. Challenge** | A novel scenario in the same domain. The visualization itself tells you whether you got it right. |

See [`examples/gradientDescent.md`](./examples/gradientDescent.md) for a worked example with profile inputs, the aha, and a live demo link.

## What it looks like under the hood

The skill ships with two files:

- **[`skill/SKILL.md`](./skill/SKILL.md)** — the entry point Claude reads on every invocation. Defines the trigger, the profiling questions, and points to the build spec.
- **[`skill/references/build-spec.md`](./skill/references/build-spec.md)**  the full build specification: how to find the "aha" for a topic, environment constraints, the 4-screen structure, component architecture rules that prevent specific React bugs, theming, performance, animation feel.

If you want to fork and modify, those are the two files to read.

## Design choices worth knowing

A few opinions baked into the skill, in case you want to argue with them:

- **Exactly 4 screens, not "3 to 5."** Flexibility was producing inconsistent artifacts. The Puzzle → Explore → Name → Challenge structure is now mandatory; if a topic needs more, it should be split into separate lessons.
- **The "aha" must be stated in one sentence before any code is written.** If Claude can't, the scope is wrong and it asks for clarification. This forces the artifact to have a point.
- **No multiple-choice quizzes inside the artifact.** Verification happens through the visualization itself if you got it, the system visibly resolves; if not, the inconsistency is visible. Quizzes are theater.
- **Anchor domain shapes scenarios, not just labels.** Choosing "medical testing" for Bayes' theorem means realistic sensitivity/specificity values and screening narratives not the same artifact with renamed variables.

## Contributing

PRs welcome. Read [`CONTRIBUTING.md`](./CONTRIBUTING.md) first — it covers how to test changes, what kinds of edits tend to be merged, and how the trigger and screen structure are intentionally rigid.

If you've used the skill on a topic where the output was bad, that's the most useful kind of issue to file. Specific failures > general feedback.

## License

[CC BY 4.0](./LICENSE). Use it, fork it, modify it, build on top of it — just credit this repo.

## Credits

Inspired by [3Blue1Brown](https://www.3blue1brown.com/)'s approach to mathematical exposition. Built as a Claude skill using Anthropic's [skills system](https://docs.claude.com/en/docs/agents-and-tools/agent-skills).

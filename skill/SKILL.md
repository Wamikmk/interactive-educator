---
name: interactive-educator
description: Build interactive educational artifacts (React JSX) that teach concepts through visual intuition and guided discovery, inspired by 3Blue1Brown. PRIMARY TRIGGER is the slash command "/aha [topic]" — when the user types this, ALWAYS trigger this skill. Also trigger on "/3b1b [topic]" (alias) and on explicit phrases like "build me an artifact for", "make an interactive explainer for", "create an interactive lesson on", "build a 3Blue1Brown-style tool for [concept]". Do NOT trigger on passive learning requests like "teach me X", "explain Y", "help me understand Z" — those are normal conversation, not artifact requests. Do NOT use for static documents, slideshows, or non-interactive text explanations.
---

# Interactive Educator

Build interactive educational artifacts that teach through visual intuition and guided discovery. The output is a single React JSX file saved to `/mnt/user-data/outputs/[topic].jsx`, then shared via `present_files`. It renders inline as a React artifact.

## Two phases, in order

### Phase 1 — Profile the learner

Before any code, use `ask_user_input_v0` to ask a small, adaptive set of multiple-choice questions. The goal is to gather just enough signal to shape the build. Do not ask what you already know from the user's prompt.

**Always ask Q1, Q3, Q5.**
**Ask Q2 only if Q1 reveals prior exposure** (total beginners have no mental model to probe; asking is theater).

If the user's opening message already answers a question (e.g., "I know calc but not linear algebra" → Q1 is partially answered), skip or narrow that question.

---

**Q1 — PRIOR KNOWLEDGE** · "What's your starting point?"
Options must be genuinely different entry points for THIS concept, not generic difficulty labels.
- BAD: "Beginner / Intermediate / Advanced"
- GOOD (eigenvalues): "Comfortable with matrix multiplication" / "Know vectors but not matrices" / "Seen the formula, don't get geometric meaning" / "Starting fresh, basic algebra only"
- GOOD (backpropagation): "Understand forward passes" / "Know chain rule but not neural nets" / "Trained models but backprop is a black box" / "Brand new to ML"

---

**Q2 — MENTAL MODEL PROBE** (conditional) · "Which best matches your current intuition?"
Skip this when Q1 indicates no prior exposure.
Options should be casual first-person explanations a learner might actually hold. Include: one roughly-correct-but-shallow, one common misconception the artifact can target, one adjacent confusion.

---

**Q3 — ANCHOR DOMAIN** · "What context would make this click?"
Domains must be places THIS concept naturally appears AND plays out differently enough to change the examples, data, and scenarios — not cosmetic skins.
- BAD: "Science / Math / Engineering"
- GOOD (Bayes): "Medical testing — screening accuracy" / "Spam filtering — how classifiers update beliefs" / "Sports analytics — updating win probability" / "Courtroom — weighing evidence"

---

**Q5 — VISUAL THEME** · "Pick a visual style:"
3–4 options, each a name + brief aesthetic + color direction. Fit the concept's mood.
- BAD: "Light / Dark / Colorful"
- GOOD (Fourier): "Oscilloscope — dark, green/cyan waveforms, retro-tech" / "Blueprint — navy, white gridlines, precise" / "Warm Analog — cream, orange/brown, hand-drawn"

---

**On pace:** default to guided/nudge style (Socratic questions visible, learner-driven). Only switch to sandbox or strict step-by-step if the user explicitly asks.

### Phase 2 — Build

Read `references/build-spec.md` in full before writing any code. It contains:
- How to find the "aha" for the concept
- Environment constraints (what libraries exist, what doesn't)
- The 4-screen structure (Puzzle → Explore → Name → Challenge)
- Component architecture and coordinate-system rules that prevent specific React bugs
- Theming, performance, animation, and tone

Then build. The learner's four inputs (prior knowledge, mental model, anchor domain, theme) must shape every screen — if the artifact would look the same regardless of the answers, you haven't used them.

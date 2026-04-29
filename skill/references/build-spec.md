# Build Specification: Interactive Educational Artifact

Build a React JSX artifact that teaches through visual intuition and guided discovery, inspired by 3Blue1Brown. Single file, saved to `/mnt/user-data/outputs/[topic].jsx` and shared via `present_files` — it renders inline as a React artifact.

## 1. Finding the "aha"

Before structure, before theme, before code: identify the ONE insight the artifact exists to deliver.

**Heuristic:** *The aha is usually the reframing a practitioner has that a textbook reader doesn't — not a fact, a perspective shift.*

Examples:
- **Eigenvectors**: *not* "vectors where Av = λv" (fact). The aha is "there are special directions in which a transformation acts like scalar multiplication — they survive the shearing." The visual discovery: most vectors rotate when you apply a matrix; these don't.
- **Bayes' theorem**: *not* the formula. The aha is "your belief should update in proportion to how much more likely the evidence is under each hypothesis." The visual: two competing hypotheses, and the evidence arrow physically tilts the probability bar.
- **Gradient descent**: *not* "subtract the gradient times learning rate." The aha is "you can find minima without ever seeing the whole landscape, just by feeling the slope under your feet." The visual: a ball on a fog-covered hill, only the local slope visible.

State the aha in one sentence before building. If you can't, the scope is wrong — ask the user which sub-aspect they want first.

## 2. Learner profile inputs

You arrive at this phase with:
- `PRIOR_KNOWLEDGE` — where to start from
- `MENTAL_MODEL` — their current (possibly wrong) intuition, or null if they're brand new
- `ANCHOR_DOMAIN` — the real-world context
- `THEME` — the visual aesthetic

If `MENTAL_MODEL` contains a misconception, the artifact's primary job is to create the conditions where the learner discovers the contradiction themselves. If it's correct but shallow, deepen it with an edge case they haven't considered. If it's null, you're teaching from a blank slate — the puzzle screen carries more weight.

## 3. Environment constraints (claude.ai artifacts)

This is non-negotiable — violating these breaks the artifact silently.

**Tailwind**: Only core utility classes (no compiler). No custom theme colors, no arbitrary values like `bg-[#abc123]`. Use inline `style={{}}` for theme-driven colors.

**No browser storage**: `localStorage`, `sessionStorage`, `indexedDB`, cookies — all forbidden. Use React state only. Data does not persist between sessions.

**Available libraries** (import these, nothing else):
- `react` (hooks via `import { useState, useEffect, useRef, useMemo, useCallback } from "react"`)
- `lucide-react` (icons)
- `recharts` (charts)
- `d3` (scales, math, simulations — not rendering)
- `mathjs`, `lodash`
- `three` (r128 — `THREE.CapsuleGeometry` does NOT exist; use CylinderGeometry + SphereGeometry)

**Single file**. Default-export a functional component with no required props.

**No fetch to external URLs** that require auth. Public open-data APIs usually work but are fragile; prefer hardcoded or generated data.

## 4. What bad output looks like — read this first

These are failure modes to avoid, not style preferences. Every item below has shipped as a real bug.

- Long text paragraphs between interactions (>2–3 sentences = lecturing, not learning)
- Quiz-style multiple choice pretending to be assessment
- Decorative visuals that don't respond to user input
- Explaining the concept fully before letting the learner explore
- Sliders or controls that don't connect to anything meaningful
- Range inputs with coarse step sizes (0.25, 0.5) — they feel broken
- Canvas with HTML overlays for click targets — coordinate mapping breaks
- Computing heatmaps or grids as SVG rect arrays (>500 elements → lag)
- Components defined inside other components — kills sliders mid-drag
- Inline arrow functions as callback props — same issue
- Using "simply" or "just" before something that isn't
- Physics animations that teleport or freeze the UI
- Using `ANCHOR_DOMAIN` superficially (just renaming variables) instead of shaping actual scenarios and data
- Theme colors that don't form a coherent palette
- Ignoring the learner profile — building the same artifact regardless of Q1/Q2/Q3/Q5

## 5. Structure — exactly 4 screens

Every screen must have:
- Something the learner can manipulate (slider, toggle, drag, click)
- One Socratic question the interaction answers
- Immediate visual feedback tied to the interaction

**Screen 1 — PUZZLE**
Open with a visual question or paradox grounded in `ANCHOR_DOMAIN`. No jargon, no definitions. A scenario the learner wants to figure out. Let them interact and form a hypothesis before anything is explained.

If `MENTAL_MODEL` contains a misconception, design the puzzle so their wrong model predicts one outcome but the interaction shows another. The gap between expectation and reality IS the teaching.

Calibrate complexity to `PRIOR_KNOWLEDGE` — reachable but not obvious.

**Screen 2 — EXPLORE**
Let the learner discover the pattern through interaction. Use Socratic questions to direct attention:
- "What happens when you drag this to the extreme?"
- "Why does this change but that stays the same?"
- "Can you make X happen? Why or why not?"

Patterns emerge from the visualization, not from reading text. Use data, scenarios, and language from `ANCHOR_DOMAIN` throughout. If the domain is medical testing, use realistic sensitivity/specificity values; if game design, game-relevant parameters.

**Screen 3 — NAME**
Now name what they've discovered. Show how the formal definition or equation captures exactly what they already observed. Every symbol or term connects back to something they interacted with.

Bridge from `MENTAL_MODEL` (if it existed) to the correct model explicitly: "You might have started thinking [their model]. The interaction showed [refined model]. Formally, this is [equation/definition]."

**Screen 4 — CHALLENGE**
A novel scenario from `ANCHOR_DOMAIN` they haven't seen. The learner applies what they've learned. The visualization itself reveals whether they're right — no multiple choice, no "check answer" buttons. If they got it, the system visibly resolves. If not, the inconsistency is visible and they can iterate.

## 6. Domain guidance

Adapt the spec to the concept's domain:
- **Math**: Always pair geometric/visual with algebraic. Every variable has a visual counterpart the learner can move.
- **Machine learning**: Use simplified but realistic data from `ANCHOR_DOMAIN` (actual numbers, scatter points), not abstract shapes. Show what the algorithm "sees" changing as parameters shift.
- **Probability**: Include simulation. Let the learner run N trials and watch empirical results converge to (or diverge from) theory.
- **Programming / CS**: Make abstract structures (trees, stacks, pointers) concrete and manipulable. Show state changes visually as they happen.

## 7. Socratic method

Questions must:
- Come BEFORE the answer, not after
- Have a definite answer discoverable through the interaction
- Build on each other — each answer raises the next question naturally
- Surface hidden assumptions ("You assumed X — what if it's not true?")

Never ask a question and immediately answer it. Pause. Let the interaction do the teaching.

## 8. Theming

Define a `THEME` object at the top of the component based on the learner's choice. Every color in the artifact references this object — zero hardcoded color literals.

```javascript
const THEME = {
  bg: '...',        // main background
  surface: '...',   // card/panel background
  text: '...',      // primary text
  muted: '...',     // secondary text, axis labels
  accent: '...',    // primary interactive color
  highlight: '...', // emphasis, correct states, key values
  grid: '...',      // gridlines, borders, dividers
};
```

Derive hover/active states by adjusting opacity or lightness of `accent` and `highlight` inline — don't add more tokens. Use the same color for the same concept across all screens (e.g., if velocity is red in screen 1, velocity is red everywhere).

Apply via inline `style={{ backgroundColor: THEME.surface }}` or by interpolating into className attributes. The theme should feel cohesive and intentional — not a color swap.

## 9. Component architecture — prevents real bugs

**Never define components inside other components.** Every render creates a new function reference; React treats it as a new component type, unmounts the old DOM element, and mounts a fresh one. This kills sliders mid-drag, resets input focus, and breaks ongoing interactions. Define ALL components at the top level of the module.

**Pass stable callbacks.** Use `useCallback` for side-effect handlers passed as props. Inline arrow functions re-create every render and cause the same unmount/remount cascade.

**Fine step sizes on sliders.** `step={0.05}` or smaller for continuous-feeling values. `step={0.25}` or `{0.5}` feels broken and prevents fine exploration.

## 10. Coordinate systems — prevents the other real bug

**For interactive visuals where the user clicks/drags to place elements, use SVG with `viewBox`.** SVG maps display coordinates to logical coordinates natively.

**Never use canvas with an absolute-positioned HTML overlay** for click targets. The mapping between canvas logical pixels, CSS display size, and absolute positioning will break on resize or zoom.

**Exception**: Use canvas for dense pixel data (heatmaps, >1000 points). Render once, overlay a transparent SVG on top for interaction. Use `pointerEvents: "none"` on the SVG and attach handlers to the parent container.

## 11. Performance

Screen transitions must feel instant:
- Never render >500 SVG elements. Switch to canvas for dense visuals.
- Heavy computation goes in `useEffect` or `useMemo`, never in the render body.
- On mount, show UI within one frame. Defer expensive work or show a skeleton.
- For per-frame animated values (physics sims), use `useRef` for mutable state and only `setState` at display-throttled intervals (every 2–3 frames), not every `requestAnimationFrame`.

## 12. Animation feel

Animations must feel physical, not computational:
- **Physics sims**: run 2–4 substeps per animation frame rather than one large step. Prevents teleporting.
- **Damping**: apply friction of 0.92–0.97 so motion decelerates smoothly.
- **Delta-time cap**: ~32ms max to prevent jumps when the tab loses focus.
- **Settle threshold**: low enough that objects visibly glide to rest, not snap.
- **Duration**: 1–3 second range. >5s is too slow to hold attention; <0.5s too fast to observe.

## 13. Tone

Precise language. Define every variable before using it. Short sentences when introducing new ideas. Wonder is earned through insight, not adjectives.

Address the learner at the level indicated by `PRIOR_KNOWLEDGE`. Don't over-explain what they already know. Don't under-explain what they don't.

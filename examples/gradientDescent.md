# Gradient Descent

> Feeling your way downhill in the fog.

**🔗 Live demo:** [add your claude.ai share link here]

A worked example of what `/aha` produces. The artifact teaches gradient descent by putting you on a foggy mountain — you can only see the slope under your feet — and asking which way you'd step.

## The aha

You can find a minimum without seeing the whole landscape — just feel the slope under your feet and step downhill.

## Profile inputs

These are the answers given in Phase 1 that shaped the build. Different answers would produce a noticeably different artifact.

| Question | Answer |
|---|---|
| Prior knowledge | Know the calculus chain rule, never seen optimization |
| Anchor domain | ML — fitting a model's parameter to minimize loss |
| Visual theme | Topographic — earth tones, contour lines, hiker's aesthetic |

(The Mental Model probe was skipped — no prior exposure to gradient descent meant nothing to probe.)

## The four screens

**1. Puzzle.** You're a hiker in heavy fog. You can see only a small arc of terrain. Drag yourself along; an orange arrow shows the direction of steepest descent at your current spot. The screen never tells you what to do — it asks: which way would you step, and how do you decide if you can't see the whole mountain?

**2. Explore.** Fog lifts. The full curve appears. You get a step-size slider and a "Step" button. Try η = 0.05 — agonizingly slow but reliable. Try η = 1.5 — you overshoot and bounce around. Try η = 2.5 — you blow up. The tradeoff between speed and stability becomes visible, not described.

**3. Name.** The rule you've been using gets its name: **gradient descent**, with formula `x_new = x_old − η · f′(x)`. Every symbol maps back to something you already moved. η is the slider you've been adjusting. f′(x) is the orange arrow. The minus sign is why you went downhill instead of up.

**4. Challenge.** A new landscape with multiple valleys. Click anywhere to drop the hiker, hit Run. From some starting points you find the deepest valley; from others you settle in a shallower one. The "why initial weights matter in ML" lesson appears as a discovery, not a claim.

## Why this artifact works

The original instinct when teaching gradient descent is to show the formula first, then explain. This artifact deliberately inverts that: by the time you see `x − η · f′(x)`, you've already been doing it for two screens. The formula is recognition, not introduction.

The fog mechanic on screen 1 is the load-bearing piece. Without it, the puzzle doesn't feel real — of course you'd walk to the lowest point, you can see where it is. With it, the question "which direction do I step?" becomes genuine, and the answer (the slope is your only signal) becomes the entire concept.

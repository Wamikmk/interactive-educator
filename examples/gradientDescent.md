# Gradient Descent

> Feeling your way downhill in the fog.

**🔗 Live demo:** [https://claude.ai/public/artifacts/9b3778bc-f30c-4537-9319-84e22c464685]

A four-screen artifact that teaches gradient descent by stranding you on a foggy mountain in a topographic-style landscape. The framing is a hiker who can only see the ground within a few meters and needs to reach the lowest valley. The entire concept is delivered through the question "which way do you step?"

## The aha

You can find a minimum without seeing the whole landscape, just by feeling the slope under your feet and stepping downhill.

## Profile inputs

These are the answers given in Phase 1 that shaped the build. Different answers would produce a noticeably different artifact.

| Question | Answer |
|---|---|
| Prior knowledge | Know the calculus chain rule, never seen optimization |
| Anchor domain | ML, fitting a model's parameter to minimize loss |
| Visual theme | Topographic, earth tones, contour lines, hiker's aesthetic |

(The Mental Model probe was skipped because no prior exposure to gradient descent meant nothing to probe.)

## The four screens

**1. Puzzle.** A hiker stands on a curve obscured by fog; only a small arc of terrain around them is visible, ringed by a dashed visibility circle. You drag the hiker along the x-axis. At every position, an orange arrow points in the direction of steepest descent, the slope under their feet. The screen never shows the full landscape, and never tells you what to do. The implicit question: how do you decide where to step when you can't see the valley?

**2. Explore.** The fog clears. The full curve is visible. You get a step-size slider (η, ranging from 0.05 to 2.5) and a "Step" button that applies one update of the rule `move downhill by η × |slope|`. A trail of past positions accumulates as you step. Try η = 0.05 and progress is reliable but agonizingly slow. Try η = 1.5 and you overshoot the valley repeatedly, bouncing between walls. Try η = 2.5 and the iterates diverge entirely. The tradeoff between speed and stability becomes visible through manipulation, not description.

**3. Name.** The rule you've been applying gets a name: **gradient descent**, with the formula `x_new = x_old − η · f′(x)` displayed prominently. The slider you've been adjusting is η. The orange arrow you've been following is f′(x). The minus sign explains why you went downhill instead of up. A live readout below the visualization shows the current values of x, η, f′(x), and Δx, the three components of the formula updating in real time as you step. The screen is doing recognition work, not introduction work.

**4. Challenge.** A new landscape appears with multiple local minima, a sinusoidal curve added to a quadratic, producing several valleys of different depths. You click anywhere to drop the hiker, then hit "Run" to let gradient descent step automatically. From some starting positions the hiker settles in the deepest valley; from others, it settles in a shallower one and stops, even though a lower point exists nearby. The visualization itself reveals when you've landed in a local rather than global minimum, and the screen surfaces the "why initial weights matter in machine learning" insight as a discovery rather than a stated fact.

## Why this artifact works

The default instinct when teaching gradient descent is to show the formula `x − η · f′(x)` first and then explain what each term does. This artifact deliberately inverts that ordering: by the time you reach screen 3, you've spent two screens *being* the formula. The slider and the arrow have already done their teaching. The formula appears as a label for an interaction you've already internalized, recognition rather than introduction. This is what makes the artifact land in a way that a derivation in a textbook usually doesn't.

The fog mechanic on screen 1 is the load-bearing piece. Without it, the puzzle isn't a puzzle, because of course you'd walk to the lowest visible point when you can see exactly where it is. The fog is what makes the question "which direction do I step?" non-trivial, and the answer (the local slope is your only signal) becomes the entire concept. Remove the fog and screen 1 collapses into a trivial geometry exercise; keep it, and the limitation forces the learner to discover the principle that the rest of the artifact formalizes.
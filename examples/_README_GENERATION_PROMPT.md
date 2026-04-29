# README generation prompt for interactive-educator examples

Paste the following prompt into the **same Claude conversation** where you just generated the artifact. Claude will already have all the context (the topic, your Phase 1 answers, what got built) — the prompt asks it to consolidate that into a markdown writeup matching the repo's example format.

After Claude returns the markdown, replace `[PASTE_CLAUDE_SHARE_LINK_HERE]` with the share link to the conversation, save the file as `examples/<topicName>.md` (camelCase, like `fourierTransform.md` or `bayesTheorem.md`), and commit.

---

## The prompt

```
Now generate a README for this artifact, in the exact format used in my interactive-educator repo's examples folder. The README will be saved as examples/<topicName>.md and serves as documentation for what got built and why.

Follow this structure precisely. Do not add sections, do not skip sections, do not change the section order.

# <Topic Name>

> <One-sentence tagline that captures the metaphor or framing of the artifact, NOT a summary of what it teaches. For gradient descent the tagline was "Feeling your way downhill in the fog." Match that level of poetry-to-precision ratio.>

**🔗 Live demo:** [PASTE_CLAUDE_SHARE_LINK_HERE]

<Two to three sentences setting up the artifact. What domain is it grounded in, what's the framing device. No marketing language — no "powerful," "illuminates," "comprehensive." Just describe what's there.>

## The aha

<One sentence. The exact aha you stated before building, verbatim if possible. If the build deviated and a different aha actually landed, state the real one — not the planned one.>

## Profile inputs

These are the answers given in Phase 1 that shaped the build. Different answers would produce a noticeably different artifact.

| Question | Answer |
|---|---|
| Prior knowledge | <answer> |
| Anchor domain | <answer> |
| Visual theme | <answer> |

<If the Mental Model probe was asked, add a fourth row. If it was skipped, add a parenthetical line below the table explaining why — e.g. "(The Mental Model probe was skipped — no prior exposure to <topic> meant nothing to probe.)">

## The four screens

**1. Puzzle.** <Describe the screen mechanically — what the user sees, what they manipulate, what question the interaction is asking them to answer. Two to four sentences. Avoid abstract phrases like "introduces the concept"; describe the actual interactive mechanic.>

**2. Explore.** <Same approach — what does the user do, what does the visual reveal, what tradeoff or pattern surfaces through interaction.>

**3. Name.** <What formal name/equation/definition appears, and which screen-2 interaction each symbol or term maps back to. The whole point of this screen is recognition, so describe what the user is recognizing.>

**4. Challenge.** <What novel scenario tests the learning, what the visualization shows when the user gets it right vs wrong, and what deeper insight or edge case the screen surfaces.>

## Why this artifact works

<Two paragraphs of meta-commentary. NOT a summary. The first paragraph identifies the specific pedagogical choice that makes the artifact land — what would a textbook do that this artifact deliberately inverts, or what mechanic carries the teaching weight. The second paragraph names the load-bearing visual or interaction element and explains what would happen without it. Be specific. Generic statements like "interactivity helps learning" are exactly what to avoid.>

---

Tone notes:
- Direct prose, no hedging. Match the matter-of-fact register of the rest of the repo.
- Past tense for what got built, present tense for what the artifact does to the reader.
- No bullet-point fragments where prose works. The screen descriptions are short paragraphs, not lists.
- Do NOT use words like: powerful, comprehensive, illuminates, journey, dive deep, leverages, seamless. They are tells of generic AI tone and clash with the repo's voice.
- Do NOT add a "Conclusion" or "Summary" section. The "Why this artifact works" section IS the closing.

Output ONLY the markdown for the README, starting with the # Topic Name header. No preamble, no closing remarks, no "here's the README:" — just the file contents.
```

---

## Workflow once you've used the prompt

1. Run `/aha <topic>` and finish building the artifact.
2. Paste the prompt above into the same conversation.
3. Claude returns the markdown. Copy it.
4. In Claude's share menu (top right of the conversation), click **Share** and copy the link.
5. In your local repo:
   ```bash
   cd ~/interactive-educator-repo/examples
   nano <topicName>.md
   ```
   Paste the markdown, replace `[PASTE_CLAUDE_SHARE_LINK_HERE]` with the share link, save (`Ctrl+O`, Enter, `Ctrl+X` to exit nano).
6. Commit and push:
   ```bash
   cd ..
   git add examples/<topicName>.md
   git commit -m "Add example: <topic name>"
   git push
   ```

That's the whole loop. Should take a minute per example once you have the rhythm.

## When to update the prompt

If you start noticing Claude's outputs drift from the format — sections in the wrong order, sneaking in marketing words, treating "Why this artifact works" as a summary — tighten the prompt with whatever it's getting wrong. The prompt is a living artifact; treat it that way.

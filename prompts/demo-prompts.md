# Demo prompts

The demo is a three-act arc. The first two acts use **no skills** — just plain prompts against the data. The third act introduces skills. The whole point is to *feel* the friction before seeing the payoff.

Total runtime if you're doing this live: about 10 minutes. Reading time only: about 4 minutes.

## What's in the inputs

- `data/march-2026-variance.xlsx` — the **starting point**. Department × line-item Budget vs Actual for March 2026.
- `data/headcount-by-dept.xlsx` — supporting context (open reqs, headcount gaps).
- `source-analysis/march-2026-findings.md` — an example of what good first-pass commentary looks like for this dataset. Use it as a reference output for Step 0, or as a shortcut input for Steps 1–3 if you want to skip the commentary generation.

The pipeline:
**data → commentary → audience-shifted versions → format-shifted deliverables**

Each arrow in that chain is its own skill. Each is also its own ad-hoc prompt that you'd otherwise type from scratch every month.

---

## Act 1 — Without skills (the megaprompt)

This is what doing the full pipeline in one shot actually looks like when you don't have skills. You sit down at the start of the close, you have all four deliverables you need, and you type — or paste from your saved snippet — a single giant prompt that spells out every spec at once.

### The megaprompt

> Here's our March variance data (`data/march-2026-variance.xlsx`) and headcount context (`data/headcount-by-dept.xlsx`). I need you to do four things in sequence.
>
> First, write up first-pass variance commentary. Working-notes style, not a polished memo. Include: a headline paragraph with the aggregate dollar and percent variance and the run-rate point if applicable; material variances (anything over $10K or 20%) each as its own paragraph with the dollar amount, cause/driver, and timing-vs-run-rate framing; two to four thematic observations that span multiple variances; specific asks naming owners where known; and any open items like reconciliation flags or late JEs.
>
> Second, take that commentary and rewrite it for our CFO. She wants something tight and decision-focused. Lead with the headline number. Three bullets, each with the dollar amount, the cause, and the implication. End with two specific asks. Cut anything that reads like internal process. Two hundred to three hundred words.
>
> Third, take the original commentary and do the same kind of rewrite for our VP of Engineering. He only cares about his department and what's coming next quarter — no other line items, focus on engineering implications.
>
> Fourth, take the CFO version and turn it into an actual email I can send. Subject line that's load-bearing (the headline takeaway in 6–8 words). Headline paragraph that opens with the aggregate number. Three structured bullets, each with the dollar amount, the cause, and the implication. Two specific asks at the end, naming the owner where known. Around 250 words for the body. Cut anything that reads like internal working notes.

### What you'll see

The model produces all four deliverables in one go. They are plausible. They are not great.

A few things start to ache as soon as you have the output in front of you:

1. **You typed ~280 words of specification to get there.** Every line of that prompt is doing real work — and you'll type some version of it every single close. Your colleague will type it slightly differently. Your future self will write it again from scratch in a year because you'll have lost the original.
2. **Iterating means retyping.** If the CFO email needs the bullets reordered, you're not just editing the email — you're either editing it manually after the fact, or running the megaprompt again with a tweak buried somewhere in the middle.
3. **The specs are invisible.** They live inside one prompt in your chat history. You can't share them with the new analyst. You can't audit what "a CFO email" means at your company. You can't reuse them for next quarter's KPI report.
4. **Output drifts month over month.** The version you write in May is going to differ from the version you write in March, because you'll restate the specs from memory each time.

This is the recurring pattern. **The moment you notice you're typing the same multi-paragraph specification twice, you should be looking at a skill.**

### Variant: run the prompts iteratively

If you'd rather see each output as it's produced (useful for live demos where you want to talk over each result), run the four pieces of the megaprompt as separate sequential prompts. The friction is the same; you just see it in slow motion. The full prompts:

> 1A. *(data files)* Write up first-pass variance commentary — working-notes style, with headline, material variances, causes, themes, asks, and open items.
>
> 1B. Now rewrite that for our CFO. Tight, decision-focused. Three bullets with dollar/cause/implication. Two specific asks. 200–300 words.
>
> 1C. Now do the same for our VP of Engineering — his department only, focused on what's coming next quarter.
>
> 1D. Now take the CFO version and turn it into an email. Subject line, structured bullets with dollar/cause/implication, two asks, ~250 words.

Either way you run it, the specs are still in the prompt — and still need to be retyped next month.

---

## Act 2 — With skills (the leveraged way)

Now imagine you wrote down those rules — the commentary structure, the audience rules, the email format — into stable files. The skills in `skills/` are exactly that. The prompts get dramatically shorter.

### Prompt 2A — commentary from the data

> Write March variance commentary from the data files in `data/`.

**What you'll see:** triggers `fpa-variance-commentary`. The model produces a working-notes writeup with headline, material variances, hypotheses, asks, open items — all in the right voice and shape. You didn't have to specify what "commentary" means.

### Prompt 2B — audience rewrite

> Rewrite that for the CFO.

**What you'll see:** triggers `fpa-audience-rewrite`. The model knows what "for the CFO" means because the skill spelled it out — three bullets, two specific asks, run-rate framing, 250 words. No re-explaining.

### Prompt 2C — different audience

> Now reframe the commentary for the VP of Engineering.

**What you'll see:** same skill, different audience persona. Output focuses only on Engineering. Doesn't mention Marketing, Sales, or G&A. You didn't have to specify any of that.

### Prompt 2D — convert to a CFO email

> Turn the CFO version into an email.

**What you'll see:** triggers `fpa-to-cfo-email`. You get a complete email — load-bearing subject line, structured headline paragraph, three bulleted variances with number/cause/implication, two specific asks. No four-sentence spec required.

### Prompt 2E — convert to a deck outline

> Build a slide outline for the monthly business review.

**What you'll see:** triggers `fpa-to-deck-outline`. Six to eight slides, each with sentence-statement title, body content, and ~100-word speaker notes. Zero typing of "what should be on each slide."

---

## Act 3 — Composition (the part that surprises people)

Skills compose. You can chain them — data to commentary to audience to format — without inventing a frankenprompt.

### Prompt 3 — full chain in one ask

> From the variance and headcount data, generate March commentary, reframe it for the VP of Engineering, and turn that rewrite into an email I can send him directly.

**What you'll see:** the model uses `fpa-variance-commentary` to produce the working notes, `fpa-audience-rewrite` to produce a VP-Eng-flavored version, then `fpa-to-cfo-email` (with the recipient adjusted) to format it as an email. Three skills, one prompt. You didn't have to specify any of the rules.

This is the payoff. Once each step lives in a skill, you can stack them by name. The number of possible end-to-end outputs goes up multiplicatively, but the typing stays at one sentence per request.

---

## How to run this live

If you have ~10 minutes:

1. Open `data/march-2026-variance.xlsx` and show the raw numbers — establish that this is just a Budget vs Actual file, no narrative yet.
2. Paste the megaprompt from Act 1. Let the model run. While it produces the four deliverables, narrate what's happening: "I am typing every spec right now. The commentary structure, the CFO rules, the VP rules, the email format — all four are inside this one prompt. Next month I get to do this again."
3. Once the output is on screen, scroll back up to the prompt and let the audience see how long it is. The visual contrast between prompt and outputs is the point.
4. Pause and pose the question: "What if all four of those specs lived somewhere stable?"
5. Open one of the SKILL.md files for ~30 seconds — point out that it's just a markdown file with the rules written down.
6. Run prompts 2A through 2E one at a time. The prompts are dramatically shorter; the outputs are at least as good as Act 1 and usually more consistent.
7. Run prompt 3 to land the composition point. Done.

The punchline writes itself: *one dataset, four skills, a full pipeline of deliverables, none of the typing.*

For a really visceral version of the demo, put the megaprompt and the four Act 2 one-liners side by side at the end. The visual contrast — one screen-filling block of specification on the left, four lines of plain English on the right, same outputs — is the strongest version of the argument.

---

## How to run this as a quick read-through

If you're not running it live and just want to see it on the page, ask the model to run prompts 1A and 2A back-to-back and put the outputs side by side. The contrast in prompt length and output consistency is enough to make the case in under two minutes.

---

## Note on the example findings doc

`source-analysis/march-2026-findings.md` is in the repo as a **reference** — it's an example of what `fpa-variance-commentary` should produce for this dataset. Useful for two things:

1. Comparing Act 1 Prompt 1A's output and Act 2 Prompt 2A's output against a known-good shape.
2. Skipping Step 0 entirely if you want to demo only the audience-rewrite and format-shift skills — just point the downstream skills at the findings file directly.

# Repurpose-with-Skills Demo

A self-contained demo showing how AI Skills automate content repurposing for analytics and finance teams. The setup uses an FP&A monthly variance review as the source content, but the pattern generalizes to any analyst's workflow — the same numbers and findings get reformatted three or four times every month for different audiences and channels.

## The idea

Repurposing is one of the highest-leverage uses of AI for analysts. The same set of numbers needs to land:

- As a tight email to the CFO
- As a slide deck for the monthly business review
- As a focused memo for the department head whose budget is being discussed
- As a board-level summary for the next board meeting

Without skills, each of those outputs starts from scratch — re-explain the audience, re-explain the format, re-paste the source content. With skills, the audience and format rules live in a reusable file, and the analyst just says "rewrite this for the CFO" or "turn this into a deck outline."

## What's in the folder

```
repurpose-work-copilot-skills-demo/
├── README.md                              ← you are here
├── data/                                  ← STARTING POINT — raw variance data
│   ├── march-2026-variance.xlsx           ← Department × Line Item Budget vs Actual
│   └── headcount-by-dept.xlsx             ← supporting headcount context
├── source-analysis/                       ← reference output (and optional shortcut input)
│   └── march-2026-findings.md             ← example of what good first-pass commentary looks like
├── skills/
│   ├── fpa-variance-commentary/SKILL.md   ← turn raw data into working-notes commentary
│   ├── fpa-audience-rewrite/SKILL.md      ← rewrite commentary for CFO / board / dept head / accounting
│   ├── fpa-to-cfo-email/SKILL.md          ← convert commentary into a CFO-ready email
│   └── fpa-to-deck-outline/SKILL.md       ← convert commentary into a slide-by-slide outline
├── prompts/
│   └── demo-prompts.md                    ← three-act demo script
├── one-pager/
│   ├── repurposing-one-pager.svg          ← editable source
│   ├── repurposing-one-pager.png          ← for image posts
│   ├── repurposing-one-pager.pdf          ← for document posts / printing
│   └── linkedin-post.md                   ← LinkedIn copy (full + short versions)
└── blog-post-draft.md                     ← stringfestanalytics.com blog draft
```

### The pipeline

The demo walks through a four-stage pipeline. Each stage is its own skill — and each is also a recurring task the analyst would otherwise re-explain in a prompt every month.

```
data → commentary → audience-shifted version → format-shifted deliverable
 ↓         ↓                  ↓                          ↓
xlsx    fpa-variance-     fpa-audience-         fpa-to-cfo-email
files   commentary        rewrite               fpa-to-deck-outline
```

The findings doc in `source-analysis/` is in the repo as a **reference output** — it's an example of what `fpa-variance-commentary` should produce for this dataset. It's also a useful shortcut input if you want to demo only the downstream skills (audience rewrite + format conversion) without generating commentary first.

## The three dimensions of repurposing

The four skills cover three distinct repurposing patterns:

1. **Data → narrative.** `fpa-variance-commentary` turns raw numbers into working-notes commentary. This is the first-pass writeup that an analyst produces after every close.
2. **Narrative → narrative (audience shift).** `fpa-audience-rewrite` reframes the commentary for a CFO, board, department head, or accounting team. Numbers stay; framing changes.
3. **Narrative → format (format shift).** `fpa-to-cfo-email` and `fpa-to-deck-outline` convert the commentary into specific deliverables. Underlying analysis stays; wrapper changes.

In real use, all three compose: data → commentary → VP-of-Engineering version → email. Act 3 of `prompts/demo-prompts.md` shows the full chain in a single prompt.

## How to run the demo

The demo is a three-act arc designed so people *feel* the friction of doing this work without skills before seeing the payoff with skills.

1. **Act 1 — The megaprompt (without skills).** Type one giant prompt with all four specs jammed in: how to structure commentary, what the CFO wants, what the VP of Engineering wants, what the email format looks like. Around 280 words of specification before any work happens. The model produces all four deliverables in one go — they're plausible, not great, and the specs are buried in your chat history forever. *(A variant with four separate iterative prompts is included for live demos that want slow-motion friction.)*
2. **Act 2 — With skills.** Run the same pipeline, but now the skills hold the rules. The 280-word megaprompt collapses into four one-liners. Outputs are at least as good and noticeably more consistent.
3. **Act 3 — Composition.** Chain the whole pipeline in a single prompt — data to commentary to audience to email. Three skills, one sentence, no frankenprompt.

Total runtime live: about 8 minutes. Reading time only: about 3 minutes. All prompts and step-by-step narration live in `prompts/demo-prompts.md`. All prompts assume the model has access to `source-analysis/march-2026-findings.md`.

The teaching point is intentional: **the moment you notice you're typing the same instructions twice, that's the cue to package as a skill.** Showing the without-skills version first makes that cue obvious.

## Why this matters for the post

For the blog post, the takeaway is:

- Repurposing is a real, recurring part of analyst work — not a one-off.
- AI is good at repurposing when the structure is well-specified.
- Skills are the right home for that structure: they live where the work lives, they don't have to be re-explained, and they compose.

The FP&A variance demo is one example. Substitute "monthly variance" with "weekly KPI report," "campaign performance review," "customer health snapshot," or any other recurring analytical artifact, and the same three-skill setup applies.

## Adapting this for other domains

If you want to fork this for a different domain, the shape stays the same:

1. Replace the source content (`source-analysis/...`) with your domain's recurring writeup.
2. Replace the audience matrix in `fpa-audience-rewrite` with the audiences your stakeholders actually use.
3. Replace `fpa-to-cfo-email` with whatever email format you actually send.
4. Replace `fpa-to-deck-outline` with whatever deck template your team uses (or add a third format skill — a one-pager, a Slack post, a newsletter section).

The skills are intentionally written so that the *structure* is the value — not the specific FP&A vocabulary.

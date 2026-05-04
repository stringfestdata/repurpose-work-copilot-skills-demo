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
├── data/
│   ├── march-2026-variance.csv            ← raw monthly variance file (Budget, Actual, Variance, %)
│   └── headcount-by-dept.csv              ← supporting headcount context
├── source-analysis/
│   └── march-2026-findings.md             ← the source content that gets repurposed
├── skills/
│   ├── fpa-audience-rewrite/SKILL.md      ← rewrite analysis for CFO / board / dept head / accounting
│   ├── fpa-to-cfo-email/SKILL.md          ← convert analysis into a CFO-ready email
│   └── fpa-to-deck-outline/SKILL.md       ← convert analysis into a slide-by-slide outline
└── prompts/
    └── demo-prompts.md                    ← six prompts to run the demo
```

## The two dimensions of repurposing

The three skills demo two orthogonal axes:

1. **Audience shift** — same format, different reader. `fpa-audience-rewrite` handles this. The numbers and structure stay roughly the same, but framing, detail level, and tone all move to match the audience.
2. **Format shift** — same content, different deliverable. `fpa-to-cfo-email` and `fpa-to-deck-outline` handle this. The underlying analysis is the same; the wrapper is an email or a deck.

In real use, these compose: an analyst might first rewrite for the VP of Engineering, then format the result as an email. Prompt 6 in `demo-prompts.md` shows the chain.

## How to run the demo

1. Make sure all three skills in `skills/` are installed in your Claude environment (Claude Code, Claude.ai, or Cowork).
2. Open `prompts/demo-prompts.md` and walk through prompts 1 through 6 in order. Each prompt assumes Claude has access to `source-analysis/march-2026-findings.md`.
3. Compare the outputs side by side. The point is to feel the difference between the same source content rendered for a CFO vs. a VP Eng vs. a board, and between an email vs. a deck.

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

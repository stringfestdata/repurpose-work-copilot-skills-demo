# Demo prompts

Six prompts to demo the repurposing skills. Each one assumes the user has the source analysis (`source-analysis/march-2026-findings.md`) and the variance CSV available, and that all three skills are installed.

The point of the demo is to show that the *same source content* produces very different deliverables depending on which skill the user invokes — and that the user doesn't have to re-explain the structure each time.

---

## Prompt 1 — Audience rewrite for the CFO

> I just finished my March variance writeup (in `source-analysis/march-2026-findings.md`). Can you rewrite this for our CFO? She wants to see this before our 1:1 tomorrow morning.

**Expected behavior:** triggers `fpa-audience-rewrite`. Output is ~250 words, leads with the headline number ($66K unfavorable on $2.1M base) and the run-rate point ($125K unfavorable per month once timing reverses), three material variances with cause and so-what, and two specific asks.

---

## Prompt 2 — Audience rewrite for the VP of Engineering

> Same writeup as before — `source-analysis/march-2026-findings.md`. Can you reframe this just for the VP of Engineering? He cares about his department only and what's coming next.

**Expected behavior:** triggers `fpa-audience-rewrite`. Output focuses only on Engineering (salary favorability, cloud overrun, open reqs). Does not mention Marketing, Sales, G&A. Includes context relevant to the VP's planning.

---

## Prompt 3 — Convert to a CFO email

> Take the March variance findings and draft the actual email I'll send to the CFO. Subject line, body, the works.

**Expected behavior:** triggers `fpa-to-cfo-email`. Output is a complete email — load-bearing subject line with the headline takeaway, two-to-three sentence opening, three bulleted variances each with number/cause/implication, two specific asks. Plain text, no markdown fences, no commentary around it.

---

## Prompt 4 — Convert to a deck outline

> I'm presenting the March variance to the leadership team on Thursday. Can you turn the findings into a slide outline I can drop into our standard template?

**Expected behavior:** triggers `fpa-to-deck-outline`. Output is a 6–8 slide outline. Each slide has a sentence-statement title (not a topic label), structured body content, and ~100-word speaker notes. The "material variances" slide describes a table with columns and rows specified.

---

## Prompt 5 — Same source, board-level rewrite

> Now do a board version of the same content — for the next board meeting deck. Strategic level, not line-item.

**Expected behavior:** triggers `fpa-audience-rewrite` with the board persona. Output is ~200 words, no line-item detail, focused on aggregate performance against commitment and forward direction. Confident tone. Doesn't mention departments by name unless directly material.

---

## Prompt 6 — Chained: rewrite then reformat

> Rewrite the March variance writeup for the VP of Engineering, and then turn that rewrite into an email I can send him directly.

**Expected behavior:** the model uses both `fpa-audience-rewrite` and `fpa-to-cfo-email` (adapting the email format for a VP-level recipient rather than the CFO). Demonstrates that skills can chain: audience first, format second. Good test of whether the model holds the audience constraint through the format conversion.

---

## How to demo this live

If you're showing this to a live audience, the quickest version is:
1. Open the source findings doc and read the headline aloud — establish that this is dense, working-doc-flavored content.
2. Run prompt 1 (CFO rewrite). Show the output side by side.
3. Run prompt 3 (CFO email). Show how the same content becomes an actual sendable artifact.
4. Run prompt 4 (deck outline). Show that the *same starting point* produces a slide deck.
5. End with prompt 6 to show chained skills.

The whole demo runs in under five minutes and the punchline is obvious: one analysis, four formats, no re-explaining.

# How to stop rewriting the same analysis for every audience

*Draft for stringfestanalytics.com/blog. Target length ~1,300 words. Voice mirrors recent posts ("How to think about technical books now that AI has the recipes," "How to stop evaluating projects in isolation"). Conversational first person, short paragraphs, comparison table, numbered conclusion.*

---

There is a chunk of analyst work that does not feel like analysis. You finish a variance review, or a campaign readout, or a quarterly KPI roll-up, and then you spend the rest of the day rewriting the same thing four different ways. The CFO gets the tight version. The department head gets the version focused on their line. The board gets the strategic frame. And all of it has to live in different formats — an email, a deck, a Slack message, a one-pager.

That part of the job is real, and it is bigger than people admit. I see finance and analytics leads spend a third of their week on it, often without thinking of it as a separate task at all. It is just "the work."

It is also exactly what AI is good at, if you set it up right.

## The pipeline behind every analytical artifact

When I look at how analysts actually take work from raw inputs to finished deliverable, almost everything I see follows the same four stages:

1. **Data → narrative.** The numbers come in. You write up commentary — what's material, what caused it, what to flag.
2. **Narrative → audience-shifted narrative.** Same content, different reader. A CFO wants three bullets and two specific asks. A department head wants context for their own line and what is coming next. A board wants the strategic picture and no line-item detail. Numbers stay constant. Framing changes.
3. **Narrative → format-shifted deliverable.** Same content, different package. The commentary becomes an email, a slide deck, a one-pager, a Slack post. Underlying analysis stays. The wrapper changes.
4. *(Loop back next month and do it all over again.)*

Stages 2 and 3 are what most people mean when they say "repurposing." But stage 1 — data to narrative — is repurposing too. It's just easier to miss because it's the part you've always written by hand.

Each stage has its own structure. Each is also predictable enough that you end up re-explaining the same rules to your AI assistant every single time you ask for help. That's the friction worth fixing.

## Why AI handles this well — when you do it right

Repurposing is structurally what large language models are best at. You are not asking the model to discover something new. You are asking it to take known content and reformat it under known constraints. That is a job description LLMs were built for.

The catch is that the quality is entirely controlled by how well you specify those constraints. "Rewrite this for the CFO" is too thin — the model will guess what a CFO wants and the result will feel generic. The fix is to be specific. The trap, which is what I want to show you next, is that "be specific" turns into "type the same specification every single time you ask."

## What this looks like without skills

To make it concrete, here is the way most analysts do this work today. The variance file just landed, and you need four deliverables before the day is out — commentary, a CFO version, a VP-of-Engineering version, and an email to send. Without skills, you sit down and type one giant prompt that spells everything out at once. Something like this:

> Here is our March variance data and headcount context. I need you to do four things in sequence.
>
> First, write up first-pass variance commentary. Working-notes style. Headline paragraph with the aggregate dollar and percent variance and the run-rate point. Material variances (anything over $10K or 20%) each as its own paragraph with dollar amount, cause, and timing-vs-run-rate framing. Two to four thematic observations. Specific asks naming owners. Any open items.
>
> Second, take that commentary and rewrite it for our CFO. Tight and decision-focused. Lead with the headline number. Three bullets with dollar amount, cause, implication. Two specific asks. Cut anything that reads like internal process. 200–300 words.
>
> Third, do the same kind of rewrite for our VP of Engineering. His department only, focus on what's coming next quarter, no other line items.
>
> Fourth, take the CFO version and turn it into an email. Load-bearing subject line. Headline paragraph opening with the aggregate number. Three structured bullets with dollar/cause/implication. Two specific asks naming owners. ~250 words.

That is around 280 words of specification, typed before any work happens.

The model produces all four deliverables. They are plausible. They are not great. And as soon as you have the output in front of you, a few things start to ache.

Iterating on one piece means re-running the whole prompt. The CFO bullets are out of order? You either edit by hand, or you tweak the megaprompt and re-run all four. The specs are now buried inside one prompt in your chat history. You cannot share them with a teammate, audit what "a CFO email" means at your company, or reuse them next quarter. Output drifts month over month, because you'll restate the specs from memory each close.

That is the pattern. The moment you notice you are typing the same multi-paragraph specification twice, you should be looking at a skill.

## Skills are the home for the rules

A Skill is a small piece of structure — usually a markdown file — that tells the model how to handle a recurring kind of task. You write it once, and the model consults it any time the task comes up.

For repurposing, this is exactly the right shape. The audience rules do not change month to month. The CFO email format does not change. The deck outline structure for monthly business reviews does not change. All of those rules belong in a Skill, not in the prompt of the day.

To make this concrete, I built a small demo around an FP&A monthly close. Four skills covering the full pipeline from raw data to finished deliverable:

- `fpa-variance-commentary` — turns the raw variance file (Budget vs Actual at the line-item level) into working-notes commentary. Headline, material variances, causes, asks, open items.
- `fpa-audience-rewrite` — reframes that commentary for a CFO, board, department head, or accounting team. Numbers stay; framing changes.
- `fpa-to-cfo-email` — converts commentary into a CFO-ready email. Subject line, headline paragraph, three structured bullets, specific asks.
- `fpa-to-deck-outline` — converts commentary into a slide outline for the monthly business review. Six to eight slides, sentence-statement titles, body content, speaker notes.

Once these skills exist, the 280-word megaprompt above collapses into four lines:

> Write March variance commentary from the data.

> Rewrite that for the CFO.

> Reframe the commentary for the VP of Engineering.

> Turn the CFO version into an email.

Same outputs. None of the specification. The model still produces working-notes commentary, a 250-word CFO email, a VP-of-Engineering rewrite that mentions only Engineering, and a deck outline ready for the monthly review — because the rules for each one live in the skill, not the prompt.

## What changes when you set this up

| Without skills | With skills |
| --- | --- |
| Re-explain the commentary structure, audience rules, and format rules on every prompt | Skills hold each set of rules; you just say "write commentary," "rewrite for the CFO," "send as email" |
| Output drifts as you re-explain slightly differently | Output stays consistent because the spec is consistent |
| Each stage is a separate, ad-hoc effort | Stages compose — data to commentary to audience to email, in one prompt |
| Hard to share the workflow with your team | The skill is the workflow — share the file, share the system |

The composition piece is the part that surprised me when I built the demo. You can ask for the whole pipeline — generate commentary from the variance file, reframe it for the VP of Engineering, send the result as an email — in a single prompt. Because each stage's rules live in its own skill, the model picks them up by reference and you get a four-stage pipeline of output without typing a four-paragraph specification.

## What this means for you

If your week has a chunk of it spent rewriting the same analysis several different ways, this is one of the highest-leverage AI applications you can set up. Not because the model is going to do your analysis — that is still your job — but because the *downstream* work has very predictable structure that you are paying for in time.

A few starting points:

1. **Pick one recurring writeup.** Variance memo, weekly KPI report, campaign readout, customer health snapshot — anything you produce on a regular cadence and rewrite for at least two audiences.
2. **Write down the rules you actually follow** when you rewrite for each audience. What do they want? What length? What gets cut?
3. **Put those rules in a Skill, not a prompt.** Even if it starts as a single markdown file with four audience descriptions, that is enough to get value immediately.
4. **Add format skills as you go.** Once you have audience covered, the same pattern works for format conversions — to-email, to-deck, to-Slack-post. Each one is a separate, small skill.

The ROI shows up within a week. Most people I talk to run their first repurposed deliverable, see the time savings, and then immediately think of three more recurring artifacts they want to wrap.

## Conclusion

If you take nothing else away from this post:

1. Repurposing is a real and underrated chunk of analytical work — and it is structurally the part AI handles best.
2. The quality of repurposing depends on how well you specify the constraints, not on how clever the model is.
3. Skills are the right home for those constraints. The rules are stable, the output gets more consistent every time you use them, and they compose.

This is a different bet than most "AI for finance" content makes. The usual pitch is that AI will do your analysis. I have a lot of doubts about that. But I have very few doubts about AI handling the rewriting and reformatting that already eats your afternoons. That part is real today, and Skills are how you keep it from drifting.

If you want to see how I put this kind of system into practice in my training, take a look [here](https://stringfestanalytics.com/work-with-me/).

---

## Editorial notes (for George — not for publication)

**Length:** ~1,790 words. Longer than your recent posts (1,200–1,400 typical) — the added length is the four-stage pipeline framing and the megaprompt walkthrough that contrasts with the four-line skill version. The megaprompt section is doing real work showing the friction visually. Two trim options if you want to bring it back to your usual range:
- **Light trim (~1,500):** Cut the four-stage pipeline section and let the friction walkthrough speak for itself. Open the post with "There is a chunk of analyst work that does not feel like analysis" and go directly to the megaprompt section.
- **Heavier trim (~1,300):** Cut the megaprompt entirely and replace with a single ad-hoc prompt (the CFO rewrite). The friction argument is weaker but the post is tighter.

**Title alternatives I'd consider:**
1. *How to stop rewriting the same analysis for every audience* (current — closest match to recent voice)
2. *How to repurpose your analysis with AI Skills (write once, deliver everywhere)*
3. *How to turn one variance file into a full month's deliverables*

**Things to verify / tweak before publishing:**
- The `[here](https://stringfestanalytics.com/work-with-me/)` link — I assumed a "work with me" page exists since recent posts close with similar links. Update to the actual destination.
- The reference to "the demo I built" in the post body — currently implied. If you want to publish the demo files publicly (GitHub link), add the link in the section "I built a small demo around an FP&A monthly close." If keeping the demo private, leave it as a hypothetical example — the post still reads cleanly.
- "Most people I talk to run their first repurposed deliverable, see the time savings…" — soft anecdotal claim. If you want to harden it, replace with a specific example from your training. If not, the current phrasing is fine.

**Tags to consider** (matching pattern of your other recent posts):
ai, ai in finance, ai tools, automation, claude skills, fp&a, knowledge work, prompt engineering, productivity, repurposing, workflow design

**Suggested promo lines** (for newsletter / LinkedIn):
- LinkedIn: "If your monthly close turns into a full week of writing the same numbers four different ways, this post is for you." → link
- Newsletter: lead with the four-stage pipeline framing (data → narrative → audience → format) — it's the most compact way to explain the full leverage of skills in a single paragraph.

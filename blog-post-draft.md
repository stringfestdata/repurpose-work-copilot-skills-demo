# How to stop rewriting the same analysis for every audience

*Draft for stringfestanalytics.com/blog. Target length ~1,300 words. Voice mirrors recent posts ("How to think about technical books now that AI has the recipes," "How to stop evaluating projects in isolation"). Conversational first person, short paragraphs, comparison table, numbered conclusion.*

---

There is a chunk of analyst work that does not feel like analysis. You finish a variance review, or a campaign readout, or a quarterly KPI roll-up, and then you spend the rest of the day rewriting the same thing four different ways. The CFO gets the tight version. The department head gets the version focused on their line. The board gets the strategic frame. And all of it has to live in different formats — an email, a deck, a Slack message, a one-pager.

That part of the job is real, and it is bigger than people admit. I see finance and analytics leads spend a third of their week on it, often without thinking of it as a separate task at all. It is just "the work."

It is also exactly what AI is good at, if you set it up right.

## The two axes of repurposing

When I look at how analysts actually rewrite their own work, almost everything I see falls into two categories.

The first is **audience shift**. Same content, different reader. The variance numbers do not change, but a CFO wants three bullets and two specific asks; a department head wants context for their own line and what is coming next; the board wants the strategic picture and no line-item detail. The numbers are constant. The framing is not.

The second is **format shift**. Same content, different deliverable. The variance findings can become an email, a slide deck, a one-pager, a Slack post. The underlying analysis is the same. The wrapper is what is changing.

Most repurposing in real work is both at once. You are rewriting the close memo *for the CFO* and turning it into an email at the same time. The combinations are where the friction is.

## Why AI handles this well — when you do it right

Repurposing is structurally what large language models are best at. You are not asking the model to discover something new. You are asking it to take known content and reformat it under known constraints. That is a job description LLMs were built for.

The catch is that the quality is entirely controlled by how well you specify those constraints. "Rewrite this for the CFO" is too thin — the model will guess what a CFO wants and the result will feel generic. Something like this is far more useful:

> Rewrite this for the CFO. Lead with the headline number. Three bullets, each with the dollar amount, the cause, and the implication. End with two specific asks. Cut anything that reads like internal process. Two hundred to three hundred words.

That produces something that lands. The problem is that I used to write that kind of long instruction every time I asked. It works, but it is not durable. You forget what you wrote last month. Your colleague writes a slightly different version. The output drifts.

## Skills are the home for the rules

A Skill is a small piece of structure — usually a markdown file — that tells the model how to handle a recurring kind of task. You write it once, and the model consults it any time the task comes up.

For repurposing, this is exactly the right shape. The audience rules do not change month to month. The CFO email format does not change. The deck outline structure for monthly business reviews does not change. All of those rules belong in a Skill, not in the prompt of the day.

To make this concrete, I built a small demo that uses an FP&A monthly variance review as the source content. Three skills, two repurposing axes:

- `fpa-audience-rewrite` — handles audience shift. Takes a variance memo and rewrites it for a CFO, board, department head, or accounting team.
- `fpa-to-cfo-email` — handles format shift to email. Subject line, headline paragraph, three structured bullets, specific asks.
- `fpa-to-deck-outline` — handles format shift to slides. Six to eight slides, each with a sentence-statement title, body content, and speaker notes.

The same March variance findings doc — about seven hundred words, dense, full of working-notes language — produces wildly different outputs depending on which skill is invoked. A two-hundred-word CFO email. A three-hundred-word VP-of-Engineering rewrite that mentions only Engineering. A seven-slide deck outline ready to drop into a template.

None of that requires re-explaining the format. The model knows because the skill knows.

## What changes when you set this up

| Without skills | With skills |
| --- | --- |
| Re-explain the audience and format on every prompt | Skills hold the rules; you just say "rewrite this for the CFO" |
| Output drifts as you re-explain slightly differently | Output stays consistent because the spec is consistent |
| Each format is a separate, ad-hoc effort | Formats compose — rewrite for the audience, then convert the result to email |
| Hard to share the workflow with your team | The skill is the workflow — share the file, share the system |

The composition piece is the part that surprised me when I built the demo. You can ask for an Engineering-flavored rewrite and then ask for that result to be sent as an email — and because the audience rule and the email rule live in two separate skills, you get both behaviors at once without writing a frankenprompt.

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

**Length:** ~1,260 words. Lands inside your usual range.

**Title alternatives I'd consider:**
1. *How to stop rewriting the same analysis for every audience* (current — closest match to recent voice)
2. *How to repurpose your analysis with AI Skills (write once, deliver everywhere)*
3. *How to turn one variance review into five deliverables*

**Things to verify / tweak before publishing:**
- The `[here](https://stringfestanalytics.com/work-with-me/)` link — I assumed a "work with me" page exists since recent posts close with similar links. Update to the actual destination.
- The reference to "the demo I built" in the post body — currently implied. If you want to publish the demo files publicly (GitHub link), add the link in the section "I built a small demo." If keeping the demo private, leave it as a hypothetical example — the post still reads cleanly.
- The 700-word source doc reference — keep this only if the demo is linked. Otherwise delete the parenthetical "about seven hundred words, dense" since readers can't see it.
- "Most people I talk to run their first repurposed deliverable, see the time savings…" — soft anecdotal claim. If you want to harden it, replace with a specific example from your training. If not, the current phrasing is fine.

**Tags to consider** (matching pattern of your other recent posts):
ai, ai in finance, ai tools, automation, claude skills, fp&a, knowledge work, prompt engineering, productivity, repurposing, workflow design

**Suggested promo lines** (for newsletter / LinkedIn):
- LinkedIn: "If you spend a third of your week rewriting the same analysis for different audiences, this post is for you." → link
- Newsletter: lead with the "audience shift / format shift" framing — it's the most compact way to explain repurposing in a single paragraph.

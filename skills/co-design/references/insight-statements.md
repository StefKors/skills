# Insight Statements method

Based on IDEO.org Design Kit, "Create Insight Statements". Insight statements succinctly articulate the most valuable learnings, the "aha" moments, that come out of research. They point the way forward and feed "How might we…" questions.

## Gather: evidence first, then fill gaps with one question round

Analyze what you can reach yourself first. The user knows things the code can't tell you, but don't interview them. Pick the one or two gaps below that matter most, and turn each into a multiple-choice question whose options are your best guesses from the evidence.

Gaps worth filling (pick, don't ask them all):
- What have you heard from customers about this feature, in their words? Any quote that stuck with you?
- What do people do instead, or around it? (Workarounds are some of the strongest signals.)
- Where do new users get stuck, and where do experienced users get stuck? Is it the same place?
- What does support or sales hear most?
- What does the data say: drop-off, usage, time to complete? What would you expect it to say?
- What works well today that we must not break?
- What surprised you when you last watched someone use it?
- Is there a group of customers this works great for? What's different about them?

Evidence sources:
- **Codebase**: walk the feature's paths. Defaults, edge cases, error and empty states, copy, feature flags, TODOs and comments that hint at known pain.
- **Running app / screenshots / folder**: walk through it as the customer, step by step. Note friction, confusion, dead ends.

Write each finding as one observation with its source (user, code, screenshot, data).

## Find themes

Cluster related findings into themes and name each theme in a few words. You don't need to show the themes separately; they surface through the insights.

## Write the insights

Rephrase each theme as a short statement of what it reveals about the customer. Not a solution; a new perspective.

A useful shape: **<customer> needs/does <X> because <underlying reason>, but <tension or barrier>.**

Insights checklist (adapted from the Design Kit worksheet):
- Is it about people's needs, motivations or behavior, not about the product's features?
- Does it reveal something non-obvious? (If everyone already knew it, it's a fact, not an insight.)
- Is it grounded in evidence you can point to?
- Does it relate directly to the problem statement?
- Does it suggest a direction without prescribing a solution?
- Would someone outside the team read it and feel "ah, that's true"?

Weak vs strong:
- Weak (fact): "Many users export reports to spreadsheets."
- Weak (solution): "Users need better filters in reports."
- Strong: "Managers export reports so they can add their own notes before sharing upward, because the report alone doesn't explain *why* the numbers moved."

Test the insights with one `multiSelect` question: "Which of these ring true?" Use one option per candidate insight, with the short statement as the label and the evidence in the description. If there are more than 4 candidates, merge the weakest or split them across two questions in the same call.

## Prioritize

Go back to the problem statement and its root cause. Drop insights that don't relate to it. Check for gaps; revisit themes if a key part of the problem has no insight. End with **3 to 5** insights.

## How might we

Turn each insight into one "How might we…" question: broad enough to allow many answers, narrow enough to be actionable.

- Too broad: "HMW improve reports?"
- Too narrow: "HMW add a notes field to reports?"
- Good: "HMW help managers tell the story behind the numbers without leaving the product?"

## Output: `02-insights.md`

```markdown
# Insights — <feature>

Problem statement: <one-line summary, link to 01>

## 1. <insight statement>
Evidence: <findings and sources>
HMW: <question>

## 2. …

## Themes considered but dropped
- <theme> — why it was dropped

_Confirmed by <user> on <date>._
```

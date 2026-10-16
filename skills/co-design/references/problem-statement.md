# Problem Statement method

Based on Figma's "How to write a problem statement" and "The 5 Whys". A problem statement is an overview of a challenge that explains the issue's cause, impact and why it matters. Its job is to stop the team jumping to solutions and to give everyone one shared definition of the problem.

In this skill, **you** do steps 1–3 from the evidence, and the user corrects you through at most two multiple-choice questions. They should spend their effort choosing, not writing.

## Step 1: Identify the problem (you)

Go look at the problem yourself: the feature's code paths, copy, defaults, error and empty states, flags and TODOs. Use the screenshots or folder if there's no repo, and the user's request and any linked issue. Look for patterns that point to something deeper than a one-off bug.

Phrase the problem as an observable effect: what happens, to whom, and how you know.

## Step 2: Put it in context (you)

Infer what you can, and mark the rest as assumptions:
- Who is affected, and in what situation (first use? at scale? a particular role?)
- What it costs: reputation, revenue, support load, churn, team time
- Whether it's a symptom of something bigger
- Earlier attempts (git history, old flags, comments, the issue thread)

## Step 3: Find the root cause with the 5 Whys (you)

Ask "why?" repeatedly, with each answer becoming the next question, until you reach a cause you can act on.

- Build **one or two candidate chains** from the evidence. Keep each answer to one sentence.
- **Stop** when a lever for change is visible. Five is a guideline: three or seven is fine.
- **Branch** when an answer has two plausible causes. That fork usually becomes your root-cause question.
- **Don't blame people.** "Someone forgot" is a symptom. Go on until the answer is about process, system, design or incentives.
- **Watch for bias.** The obvious cause is often wrong. In Figma's example the team assumed demand had risen, but the actual cause was that the AI deflection had stopped working.
- If the problem clearly has many interacting causes, say so in one line. A linear chain may oversimplify.

Show the chain as a numbered list in the statement file:

```
Problem: New users aren't using the new feature.
1. Why? → They don't notice its entry point.
2. Why? → The button is small and below the fold.
3. Why? → The screen is crowded with older controls.
4. Why? → Nothing gets removed when features are added.
Root cause: No process for retiring UI when adding features.
```

## The two questions

Ask them in a single `AskUserQuestion` call, and only about what the statement depends on most and you're least sure of. Skip any question the evidence already answers.

Example:

```json
{
  "questions": [
    {
      "header": "Who",
      "question": "Who runs into this problem most?",
      "multiSelect": false,
      "options": [
        { "label": "New admins (Recommended)", "description": "First week, setting up a workspace; most drop-off in onboarding code paths" },
        { "label": "Invited teammates", "description": "Join an existing workspace and never see the checklist" },
        { "label": "Returning users", "description": "Come back after weeks and the checklist has reset" }
      ]
    },
    {
      "header": "Root cause",
      "question": "Which root cause fits what you've seen?",
      "multiSelect": false,
      "options": [
        { "label": "Steps need missing data", "description": "Why-chain: can't finish → step needs integration → integration needs admin rights → checklist ignores roles" },
        { "label": "Value comes too late", "description": "Why-chain: stop halfway → no payoff yet → first useful result is step 5 → order follows setup, not value" }
      ]
    }
  ]
}
```

Good options:
- are specific and come from the evidence, so the user recognizes them
- differ in a way that would change the statement
- carry the reasoning in the description (the why-chain, the source)

## The statement

Write `01-problem-statement.md`:

```markdown
# Problem Statement — <feature>

> <who> is experiencing <gap> when <context>, because <root cause>. This matters because <impact / importance>.

## Gap
What is not working and what needs to be solved.

## Orientation
When and where the issue shows up; who it affects.

## Impact
Consequences for customers, the team and the business. Observable evidence.

## Importance
Why solving it matters now.

## Root cause
The 5 Whys chain and the root cause it arrived at.

## Evidence & assumptions
- Evidence: …
- Assumptions (to validate): …

## Parked ideas
Solution ideas raised during framing, held for Phase 4.

_Confirmed by <user> on <date>._
```

A good statement:
- names a specific user and situation, not "users" in general
- describes the gap, not a missing solution ("managers can't explain why the numbers moved", not "we need a notes field")
- states a root cause, not just a symptom
- is backed by at least one piece of evidence
- has a top summary short enough to read aloud in under 30 seconds

## Confirm

Ask one more `AskUserQuestion` with the top summary in a `preview`, and options such as "Confirm (Recommended)", "Change who/where", "Change root cause", "Mark more as assumptions". If they choose a change, revise that part only and confirm again.

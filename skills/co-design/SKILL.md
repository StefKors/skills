---
name: "co-design"
description: Co-design a product feature together with the user in four quick phases — a Problem Statement (root cause via 5 Whys), Insight Statements about the existing feature, a visual process map of the ideal customer flow, and clickable single-file HTML prototype directions built on the product's design system, iterated with Agentation in-browser feedback. Claude does the analysis and asks a few multiple-choice questions per phase. Use this whenever the user wants to redesign, rethink or improve a feature, frame a design problem, run design discovery, write a problem statement, do root-cause analysis on a UX issue, map a customer flow, or explore design directions or mockups for any product — even if they don't say "co-design".
---

# Co-design

This is a co-design session: you and the user think through a design problem together. The user owns the problem and every decision. You do the legwork: you investigate, analyze, draft, and build. Then you ask a few sharp questions so the user can steer.

**Propose, don't interrogate.** Open-ended questions are slow and tiring to answer. Work out what you can from the request, the codebase, screenshots or folders, then turn what you still don't know into **multiple-choice questions** whose options are your best hypotheses. The user picks one, or writes their own with "Other". Picking among good options still makes them think, because they have to reject the wrong ones. It just costs them seconds instead of paragraphs.

## Asking questions

Use the `AskUserQuestion` tool for every question:

- Ask 1–4 questions per call, each with 2–4 options. Put your recommended option first and add "(Recommended)" to its label.
- Write options as concrete hypotheses drawn from the evidence ("New admins on day 1", "Everyone after the redesign"), not as categories ("Users", "Other people").
- Keep labels to 1–5 words and put the nuance in the option's description. Use `multiSelect` when several answers can be true, such as which insights matter or which directions to build.
- "Other" is added automatically, so never add your own "Other" option.

**Show the content before you ask about it.** Not every app renders the `preview` field, so never put anything the user must read only there. Before any question that asks the user to judge something (a problem statement, insights, a flow, directions):
1. Write the file to disk.
2. Show the content as visible message text: the full draft or a tight summary of it, plus the file path. Use `SendUserMessage` when that tool is available, because text between tool calls may be summarized. Otherwise write it as plain response text right before the question.
3. Then ask. For a confirmation, also put the one-sentence summary in the question text itself, for example: `Confirm this problem statement? "New admins abandon setup because…"`. The question text always renders.

`preview` is fine as an extra, but never as the only place the content appears.

Budget per phase: **Phase 1 asks two questions at most**, plus one confirmation. Phases 2–4 use one round of questions plus a confirmation. Ask a follow-up only when an answer contradicts your draft. If the user writes a free-text "Other" answer, fold it in and move on. Don't turn it into a new round.

If `AskUserQuestion` isn't available, ask in plain text with numbered options the user can reply to with a number.

## The four phases

| # | Phase | Output | Questions |
|---|-------|--------|-----------|
| 1 | Problem Statement | `01-problem-statement.md` | ≤2 + confirm |
| 2 | Insight Statements | `02-insights.md` | 1 round + confirm |
| 3 | Ideal process map | `03-process-map.html` | 1 round + confirm |
| 4 | Prototype directions | `prototype/index.html` → `04-decisions.md` | 1 round, then Agentation feedback |

Do the phases in order and get a confirmation at the end of each one, because each phase builds on the one before. If a later phase shows that an earlier artifact was wrong, say so and offer to go back.

Save outputs in `co-design/<feature-slug>/` at the root of the product repo. Without a repo, use the folder the user pointed to or the working directory. Write each file as soon as its phase is confirmed.

## Starting

1. Read the request and look before asking:
   - **A repo is open**: find the feature in the code (routes, components, copy, states, flags, TODOs).
   - **No repo**: use the screenshots, folder or URL the user gave. If they gave nothing to look at, ask for it once, in a single question.
   - **Existing material** (an issue, research, a problem statement): read it and use it to pre-fill the phase it covers.
2. Tell the user in one or two sentences what you found and that you'll go through the four phases, then start Phase 1 right away.

---

## Phase 1: Problem Statement

Read `references/problem-statement.md` for the method and template.

Do the thinking yourself first:
1. **Identify the problem** from the request and the evidence.
2. **Put it in context**: likely who, when and where, what it costs, earlier attempts if the code or material shows them.
3. **Run the 5 Whys yourself**: build one or two plausible why-chains from the evidence down to a systemic root cause.

Then ask **at most two questions** in one `AskUserQuestion` call, aimed at whatever the statement depends on most and you are least sure of. Usually that means:
- **Who and where:** who is affected and in which situation (2–4 options).
- **Root cause:** which why-chain fits best. Each option is a root cause, and its description summarizes the chain that leads to it.

Skip a question when the evidence already answers it, so zero or one question is fine too.

Draft the Problem Statement from the answers and save `01-problem-statement.md`. Show the statement in the conversation: the summary sentence, Gap, Orientation, Impact, Importance, and the root-cause chain. Then confirm it with one more `AskUserQuestion` call whose question text quotes the summary sentence, with options like "Confirm", "Change who/where" and "Change root cause". Revise and re-confirm only if they ask for a change.

Don't propose solutions in Phase 1. Note any ideas the user volunteers under "Parked ideas" for Phase 4.

## Phase 2: Insight Statements

Read `references/insight-statements.md`.

1. Analyze the existing feature against the problem statement using the codebase, running app, screenshots or folder: friction, edge cases, empty and error states, workarounds, and what works well.
2. Draft 4–6 candidate insight statements, each with its evidence and a "How might we…" question. Show them as a numbered list in the message, each one with its statement, evidence and HMW.
3. Ask one round of questions:
   - Which insights ring true (`multiSelect`).
   - Optionally, one question on the evidence you can't get from code, such as "What do customers do instead today?", with your best guesses as options.
4. Refine down to the 3–5 chosen insights, write `02-insights.md`, and confirm.

## Phase 3: Ideal process map

Read `references/process-map.md`.

1. Draft the ideal flow from the customer's point of view: trigger, steps, decisions and goal, with each step linked to the insights it answers.
2. Visualize it. Save `03-process-map.html` (a self-contained Mermaid page) and open it for the user, or give them its path. Show it inline if you can render diagrams in the conversation. Either way, also list the steps as a short numbered list in the message, so the questions make sense even if no diagram renders.
3. Ask one round of questions about where the flow could reasonably go different ways, for example:
   - Which steps the product should do automatically, and which the customer keeps control of.
   - How an unhappy path should go.
   - Whether another person or system takes part.
4. Update the map and confirm. The confirmed steps become the screens for Phase 4.

## Phase 4: Prototype directions

Read `references/prototyping.md` before building. It covers the design system, the directions, serving the prototype and Agentation.

1. **Find the design system** in the repo (tokens, CSS variables, theme, components), or estimate it from screenshots.
2. **Propose 2–3 directions** that answer the HMW questions in genuinely different ways. Ask one round of questions:
   - Which directions to build (`multiSelect`, each with a one-line rationale).
   - If something looked uncertain, whether the design system you found is the right one.
3. **Build `prototype/index.html`** from `assets/prototype-template.html`: one file with HTML, CSS and JS, using the product's tokens and look-alike components, realistic content, working interactions, and a direction switcher that walks through the process map's steps.
4. **Serve it on localhost and connect Agentation.** From here on, the user's feedback comes as notes on elements, not as chat questions.
5. **Iterate.** Read the annotations, make the changes, and resolve each one with a summary. When a round is done, ask at most one multiple-choice question to push the comparison forward, such as "Which direction handles step 3 best?"
6. **Decide.** When the user picks or combines directions, write `04-decisions.md`: the choice and why, how it answers the problem and insights, open questions, and what to validate next with customers.

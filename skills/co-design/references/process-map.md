# Process map method

Based on Figma's "Process mapping guide". A process map visualizes every step of a process from start to finish with shapes and arrows, so a team can see the whole flow, spot gaps and align on improvements. Here it describes the **ideal outcome**: what the customer's journey looks like once the problem is solved.

## Build it

1. **Goal and scope.** State the customer's goal and the trigger that starts the flow (e.g. "Trigger: monthly numbers land → Goal: manager has shared an explained report with their director"). Scope decides the level of detail.
2. **Brainstorm steps** from the customer's perspective: actions they take, decisions they make, what the product shows or does in response. Draw on the insights and HMW questions. Include the unhappy paths that matter (errors, empty states, "not now").
3. **Choose the map type:**
   - Basic flow chart — default; one actor, start to finish.
   - Swimlane — when the customer, the product and maybe another person or system each do parts of the flow. Lanes make responsibilities visible.
   - Current vs ideal side by side — when the gap itself is the message.
   - Detailed process map — only when a step hides a subprocess worth expanding.
4. **Lay it out** with standard shapes:
   - Oval/stadium: start and end
   - Rectangle: a step or action
   - Diamond: a decision (label the branches)
   - Parallelogram: input or output (e.g. a notification)
   - Arrows show order; keep the main path straight and branches to the side.
5. **Review with the user** and simplify: remove steps the customer shouldn't have to take; that is often where the design opportunity is.

## Open forks to turn into options

Draft the whole map yourself first. Then pick the 1–3 forks below where the flow could reasonably go more than one way, and ask about them in one `AskUserQuestion` call, with your recommended version first. Don't ask about steps you can settle from the insights.

- What moment kicks this off for the customer? What were they doing just before?
- When they're done, how do they know? What do they do next?
- If this worked perfectly, what would the customer never have to think about?
- Where do they have to make a decision? What information do they need at that point?
- Which step could the product do for them? Which must stay in their hands, and why?
- What happens when it goes wrong (no data, no permission, interrupted halfway)?
- Is anyone else involved (a teammate, an approver, another system)? Should that be a swimlane?
- Which insight does this step answer? Is any insight not answered by any step?
- Compared to today's flow, which steps disappear? Which are new?

Keep it readable at a glance: roughly 5–12 main steps, short verb-first labels ("Choose a date range"), detail in annotations rather than in the boxes. Mark each step that answers an insight with its number (e.g. "↳ I2").

## Visualize it

Show it inline in the conversation when an inline diagram tool exists, and always save `03-process-map.html`. A minimal self-contained page with Mermaid:

```html
<!doctype html>
<html lang="en">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Ideal flow — FEATURE</title>
<style>
  :root { --bg:#fff; --fg:#1a1a1a; --muted:#666; }
  @media (prefers-color-scheme: dark) { :root { --bg:#111; --fg:#eee; --muted:#999; } }
  body { margin:0; padding:24px 16px; background:var(--bg); color:var(--fg); font:15px/1.5 system-ui, sans-serif; }
  main { max-width:1100px; margin:0 auto; }
  .note { color:var(--muted); }
  .mermaid { overflow-x:auto; }
</style>
</head>
<body>
<main>
  <h1>Ideal flow — FEATURE</h1>
  <p class="note">Trigger: … · Goal: …</p>
  <pre class="mermaid">
flowchart LR
  start([Trigger]) --> s1[Step one]
  s1 --> d1{Decision?}
  d1 -- yes --> s2[Step two ↳ I1]
  d1 -- no --> s3[Alternative]
  s2 --> done([Goal reached])
  s3 --> done
  </pre>
  <h2>Annotations</h2>
  <ol><li>…</li></ol>
</main>
<script type="module">
  import mermaid from "https://cdn.jsdelivr.net/npm/mermaid@11/dist/mermaid.esm.min.mjs";
  const dark = matchMedia("(prefers-color-scheme: dark)").matches;
  mermaid.initialize({ startOnLoad: true, theme: dark ? "dark" : "neutral" });
</script>
</body>
</html>
```

For swimlanes in Mermaid, use `subgraph` per actor inside a `flowchart TB`.

Ask: "Is this the journey you want customers to have? Anything missing or anything they shouldn't have to do?" The confirmed steps become the screens for Phase 4.

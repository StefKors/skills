# Prototyping with the product's design system and Agentation

## 1. Find the design system

Look before asking. In order:
- A design-system artifact or doc the user points to (or one listed as their default).
- In the repo: design tokens (`tokens.json`, `*.tokens.*`), CSS custom properties (`:root { --… }`), Tailwind config (`tailwind.config.*` theme), a component library folder (`components/ui`, `packages/design-system`), Storybook stories, theme files (`theme.ts`, `colors.*`), SwiftUI/Asset catalogs for native apps.
- The running product: inspect computed styles of real screens for colors, type scale, radius, spacing, shadows.
- Without a repo: screenshots or a linked folder. Read colors, type sizes and weights, spacing rhythm, corner radius and component shapes from them, and name tokens by role. Be explicit that these are estimates.

Mention in one line what you found (for example "Using tokens from `src/theme.ts` and the Button, Card and List components"). Only ask about it when it's genuinely unclear, such as two competing token sets, screenshots only, or an unknown platform. In that case, add it as one option-based question to the directions question round, not a separate round.

Extract into the prototype's `:root` as CSS variables: color roles (background, surface, text, muted, border, accent, danger…), font families and sizes, spacing scale, radii, shadows. Recreate the handful of components the flow needs (buttons, inputs, list rows, menus, toolbars, sheets) in plain HTML/CSS with the product's naming so the mapping back to real components is obvious. Note in an HTML comment which real component each one stands in for.

If nothing is found, tell the user and ask whether to proceed with a neutral style or wait for a source.

## 2. Structure the directions

- 2–3 directions, each a genuinely different answer to the HMW questions (e.g. "Automatic: the product does it for you" vs "Guided: the product suggests, you decide" vs "Manual, but faster: better tools, you stay in control").
- Ask which to build with one `multiSelect` `AskUserQuestion`: one option per direction, with the label as the direction name and the description giving the rationale and the insights it answers. The user can add their own direction through "Other". If they do, build it.
- Each direction gets: a short label, a one-line rationale citing insights ("Answers I1, I3"), and a walkthrough of the ideal process map steps as screens/states.
- Use realistic content (real-looking names, counts, copy), not lorem ipsum. Make the key interactions actually work in JS (click, toggle, navigate between steps); fake data is fine.
- Keep one file: `prototype/index.html`, starting from `assets/prototype-template.html`. No build step, no framework required for the prototype itself.

## 3. Serve on localhost

Agentation's toolbar syncs to its server over HTTP, and the server only accepts browser origins like `localhost` by default. A page opened as `file://` won't sync. Serve the folder:

```bash
cd co-design/<feature-slug>/prototype
python3 -m http.server 5173     # or: npx serve -l 5173
```

Then open `http://localhost:5173`. If you can open a browser yourself (built-in browser, Chrome extension, preview tool), open it and check it renders; otherwise give the user the URL.

## 4. Connect Agentation

Agentation (agentation.com) lets the user click any element in the page, write a note, and hand the agent structured context: the CSS selector, element path, computed styles and the comment. It is a React component (`agentation`, needs React 18+), so the single-file template loads React and Agentation as ES modules from esm.sh and mounts the toolbar into its own `<div>`, leaving the prototype itself framework-free. Desktop browsers only.

The template already contains this block; keep the `endpoint` in sync with the server port:

```html
<div id="agentation-root"></div>
<script type="module">
  import React from "https://esm.sh/react@18";
  import { createRoot } from "https://esm.sh/react-dom@18/client";
  import { Agentation } from "https://esm.sh/agentation@3?deps=react@18,react-dom@18";
  createRoot(document.getElementById("agentation-root"))
    .render(React.createElement(Agentation, { endpoint: "http://localhost:4747" }));
</script>
```

If the CDN import fails (check the browser console), fall back to a tiny Vite + React wrapper only around the toolbar, or have the user copy annotations from the toolbar and paste them into chat; the copy-paste output works without any server.

### MCP server (lets you read and resolve annotations directly)

Check whether `agentation_*` tools are already available. If not, the user (or you, if you have a shell on their machine) registers the server once with their agent:

```bash
claude mcp add agentation -- npx -y agentation-mcp server
npx agentation-mcp doctor   # verify
```

It listens on port 4747 by default (`--port` to change; match the `endpoint`). Restart or reload the agent's MCP servers after adding it. Node 20+ required (24 LTS recommended). Confirm the round trip: ask the user to leave one note, then list pending annotations.

## 5. Iterate on annotations

Tools: `agentation_get_all_pending` / `agentation_get_pending(sessionId)`, `agentation_acknowledge`, `agentation_reply` (ask a clarifying question in the thread), `agentation_resolve(annotationId, summary)`, `agentation_dismiss(annotationId, reason)`, and `agentation_watch_annotations` (blocks until new notes arrive, for a hands-free loop).

Loop:
1. Fetch pending annotations. Acknowledge the actionable ones.
2. Use `elementPath` to find the element in `index.html`; make the change.
3. Reload and check the result in the browser where you can.
4. Resolve with a one-line summary of what changed. Reply instead of resolving when the note is a question or ambiguous; dismiss with a reason when you and the user agreed not to do it.
5. If the user says "watch mode", call `agentation_watch_annotations` in a loop until they say stop.

Feedback now comes mainly as annotations. After a round, ask at most one multiple-choice question that moves the comparison forward. Pick one of these, with the directions as options:
- Which direction made the hardest step of the flow feel easiest?
- What should move from one direction into another?
- Which direction would your most frustrated customer pick?

Feedback that changes direction-level choices (not just details) is a signal to check the insights or process map again. Mention it rather than silently drifting.

Append each round to a "Changelog" list at the bottom of the prototype page or in `04-decisions.md`.

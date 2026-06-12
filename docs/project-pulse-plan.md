# Project Pulse — Implementation Plan

## Summary
Project Pulse is a small, static web dashboard that displays project health at-a-glance: project name, status, last update, and a set of simple metrics (progress, blockers, velocity). Goal: produce a lightweight, accessible, responsive frontend that reads a local JSON data file and renders cards/rows and a small timeline/last-updated indicator suitable for embedding in the repository or previewing in a Codespace.

---

## Scope
What will be built
- Single-page static dashboard (app/index.html)
  - Header with project name and overall status badge
  - Project summary area (lastUpdated, owner, short summary)
  - Metrics panel (list of metric name + value + small sparkline placeholder)
  - Recent changes / notes area
  - Minimal responsive layout and accessible markup
- Styling (app/styles.css) following simple design system (colors, spacing, type scale)
- Local JSON data (app/project-data.json) consumed by client-side JS embedded in index.html (no Node server required)
- VS Code launch config for easy preview in Codespaces (.vscode/launch.json)

Out of scope
- Authentication, server-side APIs, persistent storage
- Complex charts (only placeholders/simple SVGs or small inline charts)
- Full test harness / CI pipeline (only manual/local test guidance)

---

## Deliverables (files to add/modify)
- app/index.html — Single static HTML page. Purpose: structure, data fetch (app/project-data.json), JS to render UI and handle basic interactions (refresh). Expected: semantic HTML, ARIA attributes, element IDs/classes used by CSS.
- app/styles.css — Visual design and responsive CSS. Purpose: provide layout, color, type scale, accessible focus styles. Expected: CSS variables for easy theming and class hooks the Coder uses.
- app/project-data.json — Example data file the dashboard reads. Purpose: sample schema and seed data for rendering. Expected: JSON object per schema shown below.
- .vscode/launch.json — VS Code launch configuration to preview the static page (e.g., using the built-in Edge/Chrome debugger or Live Server extension). Purpose: one-click preview and debug in Codespaces; contains recommended configuration comments.

---

## Roles & Responsibilities
Designer
- Own: app/styles.css, visual mockup(s), color tokens, spacing scale, accessibility checklist.
- Deliverables:
  - High-level mockup (PNG or Figma link) or a simple wireframe image.
  - CSS variables and class naming conventions for the Coder to consume.
  - Accessibility notes (contrast ratios, keyboard order).
- Handoff: finalized styles.css + brief README snippet describing classes and variables. Work with Coder on small iterations.

Coder
- Own: app/index.html, app/project-data.json, .vscode/launch.json, small JS inside index.html to read JSON and render DOM.
- Deliverables:
  - Semantic, accessible HTML with data hooks (IDs/classes matching designer conventions).
  - project-data.json sample data and schema.
  - launch.json configured for local preview.
  - Implementation notes for how designer replaces styling or updates class hooks.
- Handoff: runbook for local testing and list of any assumptions.

---

## Implementation steps (ordered)
1. Create project-data.json (Coder) — define schema + seed data. (0.5 day)
2. Designer: produce a small mockup and CSS variables, start app/styles.css skeleton (0.5–1 day)
3. Coder: implement index.html structure and basic JS to fetch and render project-data.json using the class hooks (1 day)
4. Designer & Coder: integrate styles.css, iterate for accessibility and responsive behavior (0.5–1 day)
5. Add .vscode/launch.json and testing instructions (Coder) (0.25 day)
6. Validation and polish: accessibility checks, manual tests, adjust as needed (0.5 day)

Total estimated: 3–4 days.

---

## File assignments for each step
- Step 1: app/project-data.json — Coder
- Step 2: app/styles.css — Designer (initial skeleton); Designer provides mockup artifact in docs or PR
- Step 3: app/index.html — Coder (markup + JS)
- Step 4: app/styles.css (Designer final) + index.html (Coder small tweaks)
- Step 5: .vscode/launch.json — Coder
- Step 6: All parties review; final edits applied by Coder/Designer as agreed

---

## Dependencies
- Optional npm packages: none required for static preview. Recommended (dev):
  - live-server or http-server (npm) for local static serving
- VS Code: recommended Live Server extension or browser debugging
- Supported browsers: modern Chrome, Edge, Firefox, Safari (latest two versions)
- Node: optional for dev server — Node >=14 if used
- OS: cross-platform (Windows/Mac/Linux)

---

## Parallel work decisions
Work that can run in parallel
- Designer creating mockups + CSS variables (app/styles.css skeleton) while Coder defines JSON schema (app/project-data.json).
- Accessibility checklist and test cases can be prepared while markup is implemented.

Work that must be sequential
- Final HTML integration with final CSS requires both markup and styles to be available.
- Launch config (.vscode/launch.json) should be added after index.html exists to reference correct path.

Branching/merge strategy & coordination
- Use feature branch per task: feature/project-pulse-data, feature/project-pulse-ui, feature/project-pulse-launch.
- Small PRs, assign files to OWNER in PR description to avoid conflicts.
- Merge order: data -> UI -> styles -> launch config.

---

## Edge cases to handle
- Missing or malformed project-data.json — show user-friendly error/placeholders.
- Empty metrics array — show "No metrics" state.
- Long metric names/values — truncate with full text on hover.
- Timezones — document that lastUpdated is ISO string; display as local time.
- Large metric lists — make metrics scrollable with max height.
- Offline preview — instruct to use static server; if file:// used, fetch may fail (use fetch and fall back to inline sample).

---

## Validation expectations
Acceptance criteria
- Page loads and displays seeded data from app/project-data.json.
- Responsive: usable at 320px, 768px, 1200px.
- Accessibility: keyboard navigation works; color contrast >= WCAG AA for text.
- Data validation: JSON schema fields present; app shows clear errors for missing required fields.

Manual checks / tests
- Open index.html via static server and confirm:
  - Header shows project name and status color
  - Metrics appear with names and values
  - lastUpdated renders as local time
  - Resize window to verify layout
  - Keyboard tab order navigates interactive elements

Performance
- Page should be < 200 KB served gzipped (small static assets). No heavy libs.

---

## Suggested milestones & effort
- Milestone 1: Data + schema + basic rendering (1 day)
- Milestone 2: Designer mockup and base styles (0.5–1 day)
- Milestone 3: Styling integration + responsive fixes (0.5–1 day)
- Milestone 4: QA, accessibility, and launch config (0.5 day)

---

## Merge & deployment notes
- Test locally: run `npx http-server app` or `python -m http.server 8000` from repo root and open http://localhost:8080 or 8000/app/index.html. Alternatively use VS Code Live Server.
- .vscode/launch.json purpose: provide a one-click browser debug/preview config for Codespaces and local dev; reference app/index.html as the start page.
- Deployment: static hosting (GitHub Pages, Netlify) by publishing app/ directory.

---

## Example app/project-data.json schema
{
  "projectName": "Example Project",
  "status": "green", // green|yellow|red
  "owner": "Team A",
  "lastUpdated": "2026-06-12T06:44:14.403Z",
  "summary": "Short one-line summary of project health.",
  "metrics": [
    { "name": "Progress", "value": 72, "unit": "%" },
    { "name": "Velocity", "value": 24, "unit": "pts/week" },
    { "name": "Blockers", "value": 1, "unit": "open" }
  ],
  "notes": [
    { "date": "2026-06-11", "text": "Fixed release blockers" }
  ]
}

---

## Open questions
- Do we want inline sparklines, or just numeric metrics? (impacts design work)
- Preferred color palette or brand tokens to reuse from repo?
- Will project data later come from an API endpoint (in which case Coder should add fetch abstraction)?

---

Next steps & owners
1. Create the seed JSON (app/project-data.json) so the UI has concrete data to render — Owner: Coder.
2. Produce a one-screen mockup and the initial CSS variable set (app/styles.css skeleton) to guide markup decisions — Owner: Designer.

These two actions should start immediately and can run in parallel.

---
name: html-explainer
description: >-
  Create polished, readable, browser-openable HTML artifacts that explain
  complex work with visual structure, diagrams, annotated code, diffs, tables,
  controls, and export buttons. Use when the user asks for an HTML explanation,
  HTML artifact, visual explainer, interactive explainer, readable
  spec/report/plan, PR explainer, code walkthrough, architecture/data-flow
  visualization, design prototype, research report, custom editing interface, or
  anything that would be clearer as a self-contained HTML page than Markdown.
---

# HTML Explainer

## Overview

Use HTML as a high-bandwidth explanation format: visual, spatial, shareable, and optionally interactive. Favor a single self-contained `.html` file that the user can open locally unless the task clearly belongs inside an existing web app.

## Core Workflow

1. Clarify the artifact's job: teach, compare, review, plan, tune, or edit.
2. Gather source context from the repo, files, web, notes, logs, screenshots, or user-provided material.
3. **Load the `frontend-design` skill first when it is available**, and commit to a deliberate aesthetic direction before writing any markup. This is the single biggest lever for not shipping generic, forgettable HTML. See [Aesthetic Direction](#aesthetic-direction).
4. Design the page around fast comprehension: strong hierarchy, scannable sections, diagrams before prose when useful, and concrete examples over generic explanation.
5. Build a self-contained HTML file with inline CSS and JavaScript unless the repo already has a better local pattern.
6. Open or render the HTML when feasible and verify it is not blank, text fits, interactions work, and important visuals are visible on desktop and mobile widths.
7. Report the file path and what it covers. Mention any source context that could not be accessed.

## Aesthetic Direction

The default failure mode of generated HTML is forgettable, template-grade styling. Counter it deliberately:

- **Load `frontend-design` first.** When that skill is available, invoke it before building and follow its guidance on typography, color, motion, spatial composition, and atmosphere. Treat its craft as the baseline, then layer the comprehension-first rules below on top.
- **Serve comprehension, not decoration.** An explainer exists to be understood fast. Use distinctive design to *sharpen* hierarchy and meaning — never at the cost of readability or density. Operational/dense tools stay quiet and refined; teaching and report pages can be more expressive.
- **Pick one point of view and execute it precisely.** A clear conceptual direction (editorial, technical-minimal, terminal/mono, warm print, etc.) beats a timid mix, and varies across artifacts — do not converge on the same look every time.

If `frontend-design` is unavailable, still hold this baseline:

- **Typography:** characterful fonts over defaults (avoid Arial / Roboto / Inter / system stacks); pair a distinctive display face with a readable body face; set deliberate sizes, weights, line-height, and measure.
- **Color:** a small CSS-variable palette with a dominant color and sharp accents, used as information (severity, status, flow, grouping) rather than filler. Skip the purple-gradient-on-white cliché.
- **Motion:** one well-orchestrated load (staggered reveals via `animation-delay`) plus restrained hover/scroll states beats scattered micro-animations. Keep it CSS-only and respect `prefers-reduced-motion`.
- **Composition & atmosphere:** intentional layout (asymmetry, overlap, grid-breaks, generous whitespace *or* controlled density) and depth (gradient meshes, noise/grain, subtle borders/shadows) instead of flat solid backgrounds.

## Artifact Patterns

Choose the pattern that matches the user's goal:

- **Spec, plan, or exploration:** side-by-side options, trade-off matrix, mockups, data flow, risks, and implementation steps.
- **PR or code review explainer:** rendered diff, file map, flowchart, annotated snippets, findings by severity, and test coverage notes.
- **Architecture or code walkthrough:** module map, call/data flow, state transitions, key snippets, gotchas, and "read this first" path.
- **Design or prototype:** realistic component states, controls for timing/color/spacing/content variants, and copied parameters or prompts.
- **Report or research:** executive summary, evidence table, charts, timeline, source links, and decision-ready takeaways.
- **Custom editor:** purpose-built controls for the specific data, validation warnings, and a "copy JSON/markdown/diff/prompt" export.

## HTML Requirements

- Keep the artifact self-contained: no build step, no external network dependencies unless explicitly useful and acceptable.
- Use semantic HTML, restrained CSS, and a responsive layout. Avoid tiny text, overlapping content, and purely decorative complexity.
- Prefer real structure over long prose: tables, tabs, accordions, diagrams, timelines, checklists, callouts, annotated code, and comparison grids.
- Use SVG, Canvas, or CSS diagrams when they make relationships easier to understand. Label arrows and nodes clearly.
- Add small interactions only when they help the user reason: filters, tabs, sliders, toggles, live previews, copy buttons, draggable buckets, or expandable details.
- Include an export action for interactive editors or tuners so the user's changes can be pasted back into the conversation.
- Preserve traceability: cite local file paths, command outputs, URLs, commit IDs, or user-provided inputs when they support a claim.

## Design Guidance

- Make the first viewport useful: title, purpose, key takeaway, and navigation or summary.
- Optimize for someone who may only read once. Put the highest-value diagram, comparison, or summary near the top.
- Use color as information, not decoration: severity, status, ownership, flow direction, grouping, or state.
- For code, render snippets with line numbers or filenames when helpful and annotate the few lines that matter.
- For diffs, make additions, removals, and behavioral implications visually distinct.
- For reports, separate evidence from interpretation so the reader can trust the conclusion.
- Keep the tone appropriate to the domain: operational tools should be quiet and dense; prototypes may be more expressive.

## Verification

Before finishing, perform the strongest cheap verification available:

- Open the file in a browser when feasible. For real-browser verification, use an available
  logged-in Chrome connector such as Codex Chrome (`Chrome:control-chrome`) or Claude in Chrome,
  because it can inspect the rendered artifact in the user's actual browser session. Load and follow
  the connector's own instructions; do not hard-code environment-specific tool names here.
- If a real Chrome connector is unavailable, use Playwright/browser tooling when available.
- Check a desktop width and a narrow mobile width.
- Confirm the page renders, there are no obvious JavaScript errors, interactions work, and copy/export buttons produce useful output.
- Confirm it looks intentionally designed — distinctive type, a cohesive palette, deliberate layout — and not like a default template.
- If browser verification is not available, run a basic static check and say verification was limited.

## Trigger Phrases

Likely user phrasing includes:

- "用 HTML 解释这个"
- "make an HTML artifact"
- "生成一个可视化说明"
- "把这个 PR 做成 HTML explainer"
- "做一个可交互的调参/编辑页面"
- "把这些方案放在一个 HTML 里比较"

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
3. Design the page around fast comprehension: strong hierarchy, scannable sections, diagrams before prose when useful, and concrete examples over generic explanation.
4. Build a self-contained HTML file with inline CSS and JavaScript unless the repo already has a better local pattern.
5. Open or render the HTML when feasible and verify it is not blank, text fits, interactions work, and important visuals are visible on desktop and mobile widths.
6. Report the file path and what it covers. Mention any source context that could not be accessed.

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

- Open the file in a browser or use Playwright/browser tooling when available.
- Check a desktop width and a narrow mobile width.
- Confirm the page renders, there are no obvious JavaScript errors, interactions work, and copy/export buttons produce useful output.
- If browser verification is not available, run a basic static check and say verification was limited.

## Trigger Phrases

Likely user phrasing includes:

- "用 HTML 解释这个"
- "make an HTML artifact"
- "生成一个可视化说明"
- "把这个 PR 做成 HTML explainer"
- "做一个可交互的调参/编辑页面"
- "把这些方案放在一个 HTML 里比较"

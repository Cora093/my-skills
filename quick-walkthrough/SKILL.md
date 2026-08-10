---
name: quick-walkthrough
description: >-
  Give a concise, evidence-grounded orientation to an unfamiliar project,
  codebase, module, system, or concept. Use for a quick overview, first-pass
  mental model, explanation of how parts fit together, or representative flow
  before using, modifying, debugging, or reviewing the target. Do not use for
  durable mastery, repeated assessment, or extended tutoring; offer explicit
  $teach-me invocation for those goals.
---

# Quick Walkthrough

Build the smallest mental model that lets the user reason about the target and
continue exploring without getting lost. Optimize for useful understanding, not
coverage.

## Define Success

Finish the walkthrough when the user can explain:

1. What problem the target solves and where its boundary lies.
2. Which parts matter most and how they collaborate.
3. How one typical input, event, or action flows through the model.

Do not require the user to memorize a directory tree, API catalog, or complete
history.

## Run the Walkthrough

### 1. Establish the user's purpose

Infer the purpose when the request already says whether the user plans to use,
modify, debug, review, or simply understand the target. If the missing purpose
would materially change the route, ask one short question before exploring.
Do not begin with a questionnaire.

### 2. Gather evidence

For a local project or code target, inspect the workspace proactively. Read the
smallest useful set of entry points, documentation, configuration, core modules,
and tests. Search for facts instead of asking the user to supply facts available
in the workspace.

For a concept, decide whether the explanation depends on current versions,
standards, or external facts. Consult authoritative sources when freshness
matters; otherwise explain from stable knowledge.

Separate confirmed behavior from inference. Resolve uncertainty when cheap, and
state any uncertainty that remains.

### 3. Construct the model

Organize the explanation around this adaptable sequence:

1. Goal and boundary
2. Core parts
3. Relationships between those parts
4. One representative end-to-end flow
5. Important constraints and misleading intuitions
6. The best routes for deeper exploration

For projects, emphasize responsibilities, dependencies, control or data flow,
and the few files that anchor the model. For concepts, emphasize causal
relationships, boundaries, invariants, and counterexamples.

### 4. Deliver a one-screen first pass

Keep the initial model roughly within one screen. Lead with the central idea,
then show the map and trace one typical flow. Omit incidental details and say
what was intentionally left out when the target is large.

Use a compact Mermaid diagram or table only when it makes relationships or a
sequence materially easier to understand. Do not add a visualization merely to
decorate the answer.

After the first pass, offer a small number of meaningful routes and let the
user choose what to deepen, skip, or redirect. Deepen one route at a time.

### 5. Explain through examples

Attach a concrete example to every important relationship or abstraction. Use
real project names, inputs, and flows when available.

For an abstract or easily misunderstood idea, pair examples deliberately:

- Start with a typical example that demonstrates the model.
- Add a boundary example or counterexample that shows where the model stops
  applying.

Keep examples subordinate to the model: use them to make a relationship
predictable, not to accumulate trivia.

### 6. Ground and verify

For project walkthroughs, cite a small number of representative files and exact
line numbers. Use these as evidence anchors rather than turning the explanation
into a file inventory.

End with one lightweight transfer check. Ask the user to predict an outcome,
trace the representative flow, or assign a responsibility to the correct part.
Stop after asking and wait for the user's answer; do not reveal or explain the
answer in the same response. Correct only the important misunderstanding after
the user replies. Do not start an extended quiz or require a mastery checklist.

## Keep the Scope Fast

- Stay in the conversation by default; do not create a document or HTML artifact
  unless the user explicitly asks for one.
- Prefer one useful flow over exhaustive architecture coverage.
- Avoid lengthy prerequisites, historical background, and implementation detail
  unless they are necessary to make the model coherent.
- For durable mastery or repeated assessment, offer `$teach-me` as the explicit
  next step and wait for the user to invoke it.
- Switch to an implementation workflow when the user asks to change the target;
  orientation alone does not authorize edits.

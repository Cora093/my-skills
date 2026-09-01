---
name: quick-walkthrough
description: Concise, evidence-grounded first-pass orientation to an unfamiliar project, codebase, module, system, or concept.
disable-model-invocation: true
---

# Quick Walkthrough

Build the smallest evidence-grounded mental model that lets the user reason
about the target and continue exploring without getting lost. Optimize for
useful understanding, not coverage.

## Target Outcome

Help the user understand:

1. What problem the target solves and where its boundary lies.
2. Which parts matter most and how they collaborate.
3. How one representative input, event, or action flows through those parts.

Do not turn the walkthrough into a directory inventory, API catalog, complete
history, or mastery course.

## Workflow

### 1. Establish purpose

Infer whether the user plans to use, modify, debug, review, or simply understand
the target. Ask one short question only when the missing purpose would materially
change the route.

### 2. Gather the minimum useful evidence

For a local project or code target, inspect the workspace proactively. Read only
the entry points, documentation, configuration, core modules, and tests needed
to support the model. Search for facts available locally instead of asking the
user to provide them.

For a concept, consult authoritative sources when the explanation depends on
current versions, standards, or external facts. Otherwise use stable knowledge.
Separate confirmed behavior from inference and state unresolved uncertainty.

### 3. Construct the model

Organize the explanation around:

1. Goal and boundary
2. Core parts and their responsibilities
3. Relationships between those parts
4. One representative end-to-end flow
5. Important constraints or misleading intuitions

For projects, emphasize responsibilities, dependencies, and control or data
flow. For concepts, emphasize causes, boundaries, invariants, and useful
counterexamples.

### 4. Deliver a compact first pass

Keep the initial walkthrough roughly within one screen. Lead with the central
idea, show the small map of parts, and trace one typical flow. Use concrete names,
inputs, and examples from the target where available.

For code walkthroughs, cite a few representative files with exact line numbers
as evidence anchors. Use a compact table or Mermaid diagram only when it makes a
relationship or sequence materially easier to understand. When the target is
large, state what the first pass intentionally leaves out.

### 5. Let the user choose the next route

Offer at most a few meaningful areas to deepen, then follow the user's choice.
Deepen one route at a time instead of expanding the whole walkthrough.

## Boundaries

- Stay in the conversation by default; create no document or HTML artifact
  unless the user asks for one.
- Do not edit the target unless the user requests implementation. Switch to the
  appropriate implementation workflow when they do.
- Do not quiz or test understanding by default. Add a lightweight check only
  when the user explicitly asks to verify their understanding.
- Mention `$teach-me` only when the user explicitly wants durable mastery,
  repeated assessment, or extended tutoring.

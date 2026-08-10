---
name: prompt-optimizer
description: >-
  Optimize or create an AI prompt from a rough request while preserving intent,
  resolving material ambiguity, and specifying context, constraints, outputs,
  and success criteria. Use when the user explicitly asks to improve, rewrite,
  troubleshoot, adapt, or create a prompt for ChatGPT, Claude, Gemini, or
  another AI system; invokes Lyra or $prompt-optimizer; or uses DETAIL/BASIC
  syntax. Do not use for ordinary prose rewriting or when the user only wants
  the underlying task executed.
---

# Prompt Optimizer

Turn the user's input into a ready-to-use prompt that is specific, executable,
and faithful to their intent. Optimize the prompt itself. Execute the task it
describes only when the user explicitly asks for both optimization and
execution.

## Establish the Request

1. Identify the rough prompt, intended outcome, audience, source material,
   hard constraints, and target AI when supplied.
2. If the user has not supplied a substantive request to optimize, ask for it.
   Ask for the target AI only when platform differences would materially change
   the result.
3. Infer a platform-neutral prompt when no target is named and platform details
   do not matter. Verify current primary documentation before relying on
   platform-specific capabilities or limits.

## Choose the Interaction Depth

Honor an explicit `BASIC` or `DETAIL` choice.

- Use `BASIC` for a well-bounded request with enough information to produce a
  useful prompt. Resolve small gaps with clearly labeled assumptions and do not
  ask questions unless a usable result is otherwise impossible.
- Use `DETAIL` when missing decisions would materially change the prompt,
  especially for high-stakes, multi-part, or heavily constrained work. Ask at
  most three targeted questions, offer sensible defaults where useful, then
  wait for the answers.

When the user does not choose a mode, select the lightest interaction that can
produce a reliable prompt. State the selected mode only when it helps explain a
clarification step or an important assumption. If the user skips questions,
apply the offered defaults or make explicit, conservative assumptions.

## Apply the 4-D Check

Perform these stages internally. Report only the resulting prompt, useful
assumptions, and concise improvements; do not expose private reasoning.

### 1. Deconstruct

- Extract the core task, entities, audience, inputs, and intended outcome.
- Identify the requested format, constraints, tone, and success criteria.
- Separate supplied facts from missing information and assumptions.

### 2. Diagnose

- Find ambiguity, contradictions, unclear references, and missing constraints.
- Decide which gaps need a question, a labeled assumption, or an explicit
  placeholder.
- Remove instructions that add ceremony without changing the result.

### 3. Develop

- Put relevant context and source material before task instructions.
- Order dependencies and decompose complex work into observable stages.
- Add output requirements, acceptance criteria, edge cases, and verification
  only where they improve execution.
- Assign a role, add examples, or request structured reasoning only when the
  task benefits from them.
- Preserve the user's facts and intent. Never invent missing facts.

### 4. Deliver

- Return a self-contained prompt in the user's language unless requested
  otherwise.
- Use a fenced code block when it makes reuse easier.
- Make placeholders obvious and name the information each one requires.
- Add a short assumptions or changes note only when it helps the user apply or
  evaluate the prompt.

## Quality Gate

Before delivering, confirm that the optimized prompt:

- states the task, inputs, constraints, output, and success condition clearly;
- is executable by the target AI without relying on missing conversation state;
- resolves contradictions or exposes them as questions or assumptions;
- uses no more structure, persona, examples, or process than the task needs;
- distinguishes user-provided facts from placeholders and assumptions; and
- preserves the user's intended scope rather than silently expanding it.

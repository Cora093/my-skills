---
name: find-simplifications
description: >-
  Audit a software project for evidence-backed simplification opportunities.
  Use only when the user explicitly asks to simplify a codebase, reduce
  accidental complexity, or find removal-oriented cleanup candidates. Do not
  use for ordinary code review, bug fixing, feature work, performance or
  security audits, or formatting-only cleanup.
---

# Find Simplifications

Turn a broad simplification request into a small set of well-supported changes
that make the system easier to own. Prefer net removal and clearer behavior over
movement, renaming, or abstraction churn.

## Establish Scope And Authority

Infer whether the user wants discovery, a written proposal, or implementation.
Treat requests such as "find", "audit", and "look for" as read-only unless the
user also authorizes edits. Do not create issues, design records, comments,
branches, commits, or pull requests without that authority.

Read the repository's instructions, architecture overview, package or module
map, design records, testing policy, and relevant history before judging its
structure. Discover its source, test, documentation, generated, example, and
runtime configuration paths instead of assuming a language or directory
layout. Treat documented decisions as evidence of intent, not permanent vetoes.

Determine whether the project is a private application, a pre-release project,
or a published library. An exported API with no in-repository caller may still
have external consumers; do not call it dead without evidence about the
compatibility promise.

## Survey For Candidates

Survey broadly enough to avoid stopping at the first unused symbol. When the
user asks for breadth or many candidates, divide the repository by subsystem or
concern and use parallel subagents when available. Give each agent a bounded
area and require call-site evidence and counterevidence.

Look for these recurring forms of accidental complexity:

- Public methods, events, hooks, options, registries, helpers, packages, or
  generated artifacts with no meaningful consumer.
- Tests or documentation as the only consumers of behavior that no longer has
  a product or compatibility owner.
- Multiple representations, caches, flags, callbacks, or events mirroring the
  same fact or lifecycle transition.
- Abstractions whose implementations or consumers do not need the generality
  exposed by the interface.
- Configurability, extension points, fallback paths, or compatibility code for
  scenarios the product does not support.
- Separate modules or packages that only relocate complexity without creating
  an ownership, deployment, or dependency boundary.
- Defensive copies, validation, rollback, and hostile-input handling applied
  where typed same-process callers already own the contract.
- Hand-rolled parsers, retry loops, globbing, diffing, framing, queues, or other
  infrastructure covered by a platform primitive or healthy dependency.
- Tests, fixtures, snapshots, mocks, and generators that exist only to protect
  a candidate surface and can disappear with it.

Start with high-change or high-complexity production areas, then use static
analysis and exact searches to widen the inventory. Treat complexity metrics,
duplication tools, dead-code tools, and line counts as discovery aids rather
than conclusions.

## Prove Or Reject A Candidate

Trace each candidate before recommending it:

1. Search exact symbols, wire names, configuration keys, registrations,
   reflective lookups, generated references, and alternate spellings.
2. Classify every consumer as production, external/public, runtime
   configuration, test, documentation, generated, example, or ambiguous.
3. Read the call sites and the code that owns the behavior. Do not infer usage
   from search counts alone.
4. Check history and design records for the reason the surface exists. Verify
   whether that reason still applies to the current system.
5. State the complete deletion or collapse: implementation, interface, tests,
   docs, configuration, migration code, fixtures, and generated outputs.
6. Identify observable behavior or future flexibility that would be lost.
7. Compare net reduction against migration work, replacement glue, public API
   risk, and unrelated churn.

Reject or downgrade the candidate when a current production consumer needs it,
external compatibility is unresolved, the proposal only moves complexity, the
evidence is speculative, or the removal is a product decision outside the
user's scope. A reasonable behavior change can still be a simplification, but
label it explicitly rather than presenting it as dead-code cleanup.

Do not elevate typo fixes, stylistic preferences, arbitrary deduplication, or
"this looks complex" observations into durable proposals. Include a small local
cleanup only when it is actionable and clearly safe.

## Audit Ownership And Lifecycle Machinery

For copies, freezes, validators, caches, and retained callbacks, identify where
the value originates, whether the recipient borrows or owns it, and which
boundaries can introduce untrusted or delayed data. Preserve validation at
parsers, configuration loading, persistence, queues, worker or process
messages, model or tool data, and network or wire decoding. Challenge redundant
defenses between trusted, typed, same-process collaborators.

For asynchronous code, map each state flag, readiness signal, cancellation
path, terminal outcome, and disposer to an owner and transition. Collapse
mechanisms that encode the same fact. Preserve distinct machinery when it
protects publication and rollback, callback containment, first-outcome
arbitration, resource ownership, or disposal reaching quiescence.

## Evaluate Standard And Dependency Replacements

Treat replacing custom infrastructure as a candidate only when it reduces code
the project must understand and test. Confirm the replacement covers the exact
semantics, supports the project's runtime floor, is maintained, has an
acceptable dependency and security footprint, and leaves little glue. Prefer a
platform primitive when it meets the same bar. Reject a wrapper that merely
moves the custom behavior behind a dependency.

## Present The Result

Prefer a few high-confidence candidates over a long inventory of guesses. Rank
them by evidence strength, net maintenance reduction, behavioral risk, and
implementation independence. For each candidate, report:

- The concrete surface and its location.
- Current consumers and counterevidence checked.
- What can be removed or collapsed, including follow-on artifacts.
- What behavior, compatibility, or flexibility is given up.
- Why the result is simpler rather than merely different.
- Confidence, unresolved questions, and the smallest verification plan.

Report rejected near-misses when they prevent an attractive but incorrect
deletion, especially for public APIs, security checks, and lifecycle controls.
State which areas were surveyed and which were intentionally excluded.

Use the repository's existing proposal format only when the user requests an
artifact or local rules require one for the authorized change. Otherwise keep
the audit in the conversation. Consolidate with an existing issue, ADR, RFC, or
design record rather than creating a duplicate.

## Implement And Verify

When implementation is authorized, prefer independent candidates that produce
coherent diffs. Remove the entire obsolete surface and update its callers,
tests, documentation, configuration, generated files, and design records
together. Do not preserve compatibility shims that negate the simplification
unless the project's compatibility policy requires them.

Run focused checks chosen from the outgoing diff and the repository's own
instructions. Verify the behavior that remains, search again for deleted names
and stale documentation, inspect the final diff for accidental scope growth,
and distinguish static or test validation from real-runtime acceptance when the
latter matters.

# Project state schema

Create this structure at the project root:

```text
.ai-state/
|-- PROJECT.md
|-- CURRENT.md
|-- checkpoints/
|   `-- NNNN-slug.md
|-- records/
|   `-- NNNN-slug.md
`-- decisions/
    `-- NNNN-slug.md
```

Create subdirectories lazily when their first entry is needed. Number each
directory independently by scanning its highest existing number and incrementing
it. Use dash-case slugs.

## PROJECT.md

`PROJECT.md` holds the durable frame. Revise it when the project goal, scope, or
constraints change.

```md
# Project: {name}

## Purpose

{The concrete outcome this project exists to produce.}

## Success criteria

- {Observable outcome}

## Scope

- {Included responsibility}

## Constraints

- {Technical, operational, schedule, compatibility, or user constraint}

## Architecture

{Only the context that cannot be recovered cheaply from the repository. Link to
authoritative architecture documents when they exist.}
```

## CURRENT.md

`CURRENT.md` is the authoritative handoff snapshot. Keep it concise and replace
stale statements rather than accumulating a journal.

```md
# Current project state

Updated: {ISO-8601 timestamp with timezone}
Status: active | blocked | completed

## Objective

{The current concrete objective.}

## Verified progress

- {Completed outcome and its evidence}

## Current behavior

{What the repository or system actually does now.}

## Next actions

1. {The next executable action}

## Blockers

- None

## Verification

- `{command or check}` — {result}

## Relevant files

- `{path}` — {why it matters}

## Pointers

- Latest checkpoint: `checkpoints/NNNN-slug.md`
- Active record: `records/NNNN-slug.md`
- Accepted decision: `decisions/NNNN-slug.md`
```

Omit empty pointer categories. For a blocker, state the exact missing input or
external condition and what becomes possible after it is resolved.

## Checkpoints

A checkpoint records a verified milestone, not every session or shell command.
Write one when a coherent work unit is completed, a handoff boundary is reached,
or the implementation changes phase.

```md
# {Milestone}

Date: {ISO-8601 timestamp with timezone}
Status: completed | partial | blocked

## Outcome

{What changed and what now works.}

## Evidence

- `{test, command, log, commit, issue, or file}` — {result}

## Remaining

- {Work deliberately left for the next checkpoint}
```

## Records

A record captures a non-obvious, durable finding supported by evidence. Write one
for a confirmed root cause, important system behavior, rejected investigation
path worth avoiding, or user-established requirement that changes future work.

```md
# {Verified finding}

Date: {ISO-8601 timestamp with timezone}
Status: active

{What was established and why it matters.}

## Evidence

- `{test, log, issue, or code path}` — {what it demonstrates}

## Implications

- {How this changes future analysis or implementation}
```

When later evidence replaces the finding, keep the old record and set:

```text
Status: superseded by REC-NNNN
```

## Decisions

A decision records a consequential technical choice. Small, easily reversible
implementation details remain in code and tests.

```md
# {Decision title}

Date: {ISO-8601 timestamp with timezone}
Status: accepted

## Context

{The forces and constraints requiring a decision.}

## Decision

{The chosen approach.}

## Rationale

{Why this option best fits the constraints.}

## Consequences

- {Operational, implementation, testing, or maintenance effect}
```

When another decision replaces it, keep the old decision and set:

```text
Status: superseded by ADR-NNNN
```

## Evidence and maintenance rules

- Separate verified facts from hypotheses. Put an unverified item in
  `CURRENT.md` as pending work until evidence promotes it to a record.
- Prefer stable pointers such as repository paths, issue URLs, and commit IDs.
- Update a stale `CURRENT.md` immediately after checking repository reality.
- Preserve history through checkpoints and supersession; keep the current
  snapshot small.
- Store reasoning that cannot be reconstructed cheaply. Let code, configuration,
  tests, and version control retain mechanically recoverable detail.

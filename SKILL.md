---
name: project-state
description: Persist, restore, and hand off software-project context across AI sessions. Use when initializing project state, resuming multi-session development, recording verified milestones or findings, or preparing a durable development handoff.
---

# Project State

Maintain a compact, evidence-backed state layer in the project root at
`.ai-state/`. Its purpose is to let a fresh agent continue without relying on
chat history. Repository code, configuration, tests, and runtime evidence remain
the source of truth; state files preserve intent, verified conclusions, and
navigation pointers.

Read [references/state-schema.md](references/state-schema.md) when initializing
the state directory, adding a checkpoint, record, or decision, or changing a
file's status.

## Resume

At the start of work:

1. Resolve the project root from the repository when possible; otherwise use the
   current workspace root.
2. If `.ai-state/` exists, read `PROJECT.md` and `CURRENT.md`, then read the
   active records, decisions, and latest checkpoint referenced by `CURRENT.md`.
3. Inspect the relevant code, configuration, tests, and Git working state. Reconcile
   stale state files with repository evidence before relying on them.
4. State the recovered objective, verified progress, blockers, and immediate next
   action briefly, then continue the user's requested work.

Resume is complete when the current objective and next action are supported by
both the persisted state and current repository evidence.

## Initialize

When `.ai-state/` is absent and this skill was explicitly invoked, or the user
asked for durable project tracking:

1. Inspect the repository's existing documentation, configuration, code layout,
   Git state, and tests.
2. Create the structure and initial files defined in
   [references/state-schema.md](references/state-schema.md).
3. Populate only facts supported by the repository or stated by the user. Mark
   material uncertainty as `Unknown` or `Pending confirmation`.
4. Ask for missing information only when it blocks the requested work or would
   materially change its direction.

Initialization is complete when `PROJECT.md` captures the durable project frame
and `CURRENT.md` gives a fresh agent a concrete next action.

## Record

Keep the state layer high-signal:

- Update `CURRENT.md` after a meaningful phase transition, when the objective or
  blocker changes, and before ending a substantial work session.
- Append a checkpoint for a verified milestone that another agent may need to
  distinguish from the current snapshot.
- Append a record for a durable, evidence-backed finding that changes later
  analysis or implementation.
- Append a decision for a consequential technical choice with meaningful
  alternatives or downstream effects.
- Reference code, tests, issues, commits, records, and decisions by path or URL
  rather than copying their contents.
- Preserve superseded records and decisions by status and successor reference.
- Keep hypotheses explicitly pending until evidence confirms or rejects them.
- Keep credentials, tokens, personal data, and other secrets out of state files.

The state layer is a navigation and reasoning index, not a command transcript.
Routine exploration and facts cheaply recoverable from one source file do not
need durable records.

## Close a work unit

Before reporting a substantial task complete:

1. Run verification proportional to the change and capture the result in
   `CURRENT.md`.
2. Update completed work, actual current behavior, remaining actions, blockers,
   relevant files, and pointers.
3. Write any qualifying checkpoint, record, or decision, then link it from
   `CURRENT.md`.
4. Check that a fresh agent could identify the next executable action without
   access to the conversation.

Treat the state files as ordinary project files. Preserve unrelated user changes
and leave committing or publishing them to the user's requested workflow.

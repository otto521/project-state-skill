# Project State Skill

A Codex skill for preserving software-project context across AI sessions.

It maintains a small, evidence-backed `.ai-state/` directory so a fresh agent
can recover the project's objective, verified progress, decisions, findings,
blockers, and next actions without relying on chat history.

## State layout

```text
.ai-state/
├── PROJECT.md       # Long-term purpose, scope, and constraints
├── CURRENT.md       # Current objective, progress, blockers, and next actions
├── checkpoints/     # Verified development milestones
├── records/         # Durable evidence-backed findings
└── decisions/       # Consequential technical decisions
```

Repository code, configuration, tests, and runtime evidence remain the source of
truth. The state directory acts as a compact navigation and reasoning index.

## Install

```bash
git clone https://github.com/otto521/project-state-skill.git \
  ~/.codex/skills/project-state
```

Restart Codex after installation so the skill is discovered.

## Usage

Initialize state in a project:

```text
$project-state Initialize durable project state, then continue my development task.
```

Resume in a new session:

```text
$project-state Restore the project state and continue the next action in CURRENT.md.
```

The skill also supports automatic discovery for multi-session development,
verified milestone recording, and durable handoffs.

## Repository contents

- `SKILL.md` — recovery, initialization, recording, and handoff workflow.
- `references/state-schema.md` — state-file schemas and templates.
- `agents/openai.yaml` — Codex UI metadata and default invocation prompt.

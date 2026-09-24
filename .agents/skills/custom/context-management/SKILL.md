---
name: context-management
description: Maintain recoverable state for long-running AI development work across context growth, compression, interruptions, and handoffs.
---

# Context Management

Use this skill when a task spans multiple stages, touches several areas, may outlast the current context, or has decisions that must survive interruption.

## Source of truth

Use the project files:

- .ai/current-task.md for the active task and immediate progress.
- .ai/decisions.md for important decisions that should remain valid beyond the current task.

Do not use either file as a dump for every thought or routine edit.

## Start or resume a long task

1. Read the relevant project instructions and inspect the current working tree.
2. Read .ai/current-task.md and .ai/decisions.md before making new plans.
3. Compare the recorded state with the repository and current user request.
4. Preserve confirmed decisions unless new evidence, constraints, or user direction requires reconsideration.
5. If the recorded state is stale, correct it before continuing.

## Maintain current-task.md

For a complex task, keep these sections meaningful:

- Goal: the outcome, not a list of implementation steps.
- Status: a short state such as planning, in progress, blocked, or ready for verification.
- Completed: finished work with enough detail to avoid repeating it.
- In Progress: the active work and current evidence.
- Pending: remaining work or decisions.
- Constraints: user requirements, compatibility limits, scope boundaries, and known permissions.
- Decisions: decisions made for this task; link to decisions.md when they are long-lived.
- Verification: checks already performed and their results.
- Known Problems: unresolved failures, uncertainty, or environmental limitations.
- Next Step: the smallest concrete next action.

Update the file at meaningful boundaries: after planning, after a substantial implementation step, after verification, and before handing off or stopping.

## Maintain decisions.md

Record a decision only when it is likely to affect future work or multiple tasks. Include context, the reason for choosing it, alternatives considered, and impact.

Do not record a temporary debugging hypothesis as an architecture decision. If a decision is reversed, keep the history understandable by adding a new dated decision rather than silently rewriting the old one.

## After context compression or interruption

Recover in this order:

1. Read AGENTS.md and any more-specific project instructions.
2. Read .ai/current-task.md.
3. Read the relevant entries in .ai/decisions.md.
4. Inspect the current working tree and the files named by the state.
5. Verify that the recorded completed work still exists before continuing.

Continue from Next Step and Pending. Do not restart analysis from zero or re-implement Completed work unless repository evidence shows it is absent or invalid.

## Handoff format

Before stopping a long task, leave:

- the current status;
- what changed and what was verified;
- unresolved problems and their evidence;
- decisions that must not be casually revisited;
- the next action a future agent should take.

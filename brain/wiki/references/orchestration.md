---
tags: [orchestration, agents, roles]
---
# Orchestration Reference

> Detailed orchestration flows for the agent team. Summary in root CLAUDE.md → "Agent Team & Roles."

## Orchestration via Claude Code Subagents

The main conversation acts as **Orchestrator**. It spawns role agents as subagents using the Agent tool. Key rules:

1. **Implementer and Reviewer are always separate agents** — the Reviewer must NOT see the Implementer's reasoning. This is the whole point. Fresh context = fresh eyes
2. **Load the role CLAUDE.md into the agent prompt** — each subagent gets its role definition + the task file as input
3. **Parallel when independent** — multiple Implementer agents can run in parallel for independent tasks within a slice
4. **Sequential when dependent** — Implementer must finish before Reviewer starts

## Milestone-Level Flow

```
User gives directive
    |
1. RESEARCH — Orchestrator or Planner agent investigates
   |-- check wiki, playbook, retrospectives for prior art
   |-- identify unknowns, risks, constraints
   |-- fill Research section in milestone file
   +-- set phase: plan
    |
2. PLAN — Planner agent decomposes
   |-- read Research output
   |-- create slices and tasks with Read/Write/Verify contracts
   |-- auto-suggest skills per task
   +-- set phase: execute
    |
3. EXECUTE — per task within each slice:
   |-- read task file, determine team-size, load skills
   |-- check blocked-by: skip if any blocker is incomplete
   |-- spawn Implementer agent
   |       (input: role CLAUDE.md + skills + task file + project CLAUDE.md)
   |-- spawn Reviewer agent -> writes Verification
   |       (input: role CLAUDE.md + skills + task file + output files ONLY)
   |       (does NOT receive Implementer's conversation)
   +-- if fail -> re-spawn Implementer with feedback
       if pass -> /done
    |
4. COMPLETION — all slices verified
   |-- check milestone acceptance criteria
   |-- signal tag + retrospective if non-routine
   |-- merge milestone branch to main, delete milestone branch
   +-- set phase: completion
```

## What Each Agent Receives

### Implementer
- Role CLAUDE.md (`_roles/implementer/CLAUDE.md`)
- Task file (Goal, Read, Write, Verify)
- Matched skills from task's `skills` field
- Project CLAUDE.md (context)
- Wiki pages from project's `required_reading`

### Reviewer
- Role CLAUDE.md (`_roles/reviewer/CLAUDE.md`)
- Task file (especially Verify section)
- Output files listed in Write section — **the actual files, not the Implementer's conversation**
- Matched skills (same as Implementer)
- Does NOT receive: Implementer's conversation, reasoning, or Progress notes

### Planner
- Role CLAUDE.md (`_roles/planner/CLAUDE.md`)
- Milestone file
- Wiki index + playbook
- Skills catalog (`skills/index.md`)
- Project CLAUDE.md

## Parallel Execution

Within a slice, tasks that have no `blocked-by` dependencies on each other can be executed in parallel:

```
Slice: "Update wiki pages"
  Task A: "Update page 1"  blocked-by: []      ← can run in parallel
  Task B: "Update page 2"  blocked-by: []      ← can run in parallel
  Task C: "Update index"   blocked-by: [A, B]  ← must wait for A and B
```

The Orchestrator spawns Implementer agents for A and B simultaneously, then runs C after both pass verification.

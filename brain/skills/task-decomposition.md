---
name: "task-decomposition"
description: "How to break work into Milestone/Slice/Task correctly with proper sizing"
source: brain
triggers: ["#planning", "#decomposition", "milestone", "backlog"]
tags: [planning, brain-system]
---
# Skill: Task Decomposition

## When to Use
When decomposing a new project, milestone, or large piece of work into executable units. Required reading for the Planner role.

## Key Principles
1. **Slices deliver standalone value.** If you stop after a slice, the system is coherent. No half-built states.
2. **Tasks fit the 50% rule.** Read list ≤10 files. Goal in one sentence. If not, split.
3. **Verify criteria are mechanically checkable.** "Looks good" is not a criterion. "File X contains Y" is.
4. **Sequence minimizes dependencies.** Independent tasks first, dependent tasks after.
5. **Each task has a single concern.** If you say "and" in the goal, it's probably two tasks.

## Procedures

### Decomposing a Milestone
1. Define the outcome — what's true when the milestone is done?
2. Identify 2-5 slices that each deliver a coherent piece of that outcome
3. Order slices so each builds on the last but remains independently valuable
4. Write acceptance criteria for the milestone — these are integration-level checks

### Decomposing a Slice into Tasks
1. List everything that needs to change (files, configs, docs)
2. Group changes by dependency — things that must happen together = one task
3. For each task, write the Read/Write/Verify contract
4. Check the 50% rule on each task's Read list
5. Assign team-size: routine/low-risk → solo, standard → pair, complex → full
6. Order tasks to minimize blocking

### Validating a Decomposition
- [ ] Every slice has standalone value
- [ ] Every task goal fits one sentence
- [ ] Every task Read list is ≤10 files
- [ ] Every Verify criterion is mechanically checkable
- [ ] No circular dependencies between tasks
- [ ] team-size matches risk level

## Anti-Patterns
- **God task** — one task that does everything. Split it.
- **Horizontal slicing** — "do all the backend, then all the frontend." Slice vertically instead.
- **Verify-less tasks** — no acceptance criteria means no definition of done. Always have at least one.
- **Speculative decomposition** — planning tasks for features that haven't been confirmed. Decompose just-in-time.
- **Over-decomposition** — 50 tiny tasks with 2-line Read lists. If it takes longer to describe than to do, merge it.

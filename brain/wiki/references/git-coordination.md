---
tags: [git, coordination, branching]
---
# Git Coordination Spec

> Canonical reference for branch strategy, commit conventions, and crash recovery. Referenced by root CLAUDE.md → "Git Coordination."

## Branch Strategy

```
main                              ← stable, all milestones completed and verified
├── milestone/self-learning       ← created at phase: execute
├── milestone/skill-system        ← one branch per active milestone
└── (worktrees for tasks)         ← temporary, created and destroyed per task
```

Three levels only:
- **main** — always in a verified, coherent state. Only receives merges from completed milestones.
- **milestone/\<name\>** — created when a milestone enters `phase: execute`. All slice/task work happens here. Deleted after merge to main.
- **Worktrees** (not branches) — temporary isolated copies for Implementer agents. Created per-task, destroyed after merge or discard. Claude Code's `isolation: "worktree"` handles this natively.

No slice branches, no task branches. Markdown repos have rare merge conflicts — heavy branching adds overhead without proportional value.

## When Branches Are Created/Destroyed

| Event | Git Action |
|-------|-----------|
| Milestone enters `phase: execute` | `git checkout -b milestone/<name>` from main |
| Task activated (pair/full) | Orchestrator spawns agent with `isolation: "worktree"` |
| `/save` | Commit on current branch (milestone or worktree) |
| `/verify` pass | Merge worktree to milestone branch, delete worktree |
| `/verify` fail | Keep worktree alive, Implementer reworks in same worktree |
| `/done` | Commit with signal tag in message |
| Slice complete | All tasks merged, commit on milestone branch |
| Milestone `phase: completion` | Merge milestone branch to main, delete milestone branch |

## Worktree Usage by Team Size

| Team Size | Git Behavior |
|-----------|-------------|
| **solo** | Work directly on milestone branch. No worktree overhead for low-risk tasks. |
| **pair** | Implementer in worktree. Reviewer reads the worktree diff. Pass → merge. Fail → Implementer reworks in same worktree. |
| **full** | Planner commits decomposition on milestone branch. Implementer in worktree. Reviewer checks diff. Same merge/fail flow as pair. |

## Commit Convention

```
[milestone/<name>] <type>: <description>
```

**Types:**
- `research` — Research phase output
- `plan` — Decomposition, task contracts created
- `task` — Task execution (include task name)
- `slice` — Slice completion
- `save` — Checkpoint via /save
- `completion` — Milestone completion

**Examples:**
```
[milestone/self-learning] research: current state analysis and risk assessment
[milestone/self-learning] plan: decomposed into 3 slices, 8 tasks
[milestone/self-learning] task: update done.md format #routine
[milestone/self-learning] task: add /review command #had-friction
[milestone/self-learning] slice: tier-1-signals verified
[milestone/self-learning] save: checkpoint mid-task, resume from CLAUDE.md update
[milestone/self-learning] completion: all 3 slices verified, merged to main
```

Signal tags (`#routine`, `#had-friction`, etc.) go in the commit message for task commits — makes `git log` a learning signal too.

## How Commands Interact with Git

### /save
1. Update task/slice/milestone files (existing behavior)
2. Stage all changes in `brain/`
3. Commit: `[milestone/<name>] save: <resume-from note>`

### /done
1. Run `/save` (which commits)
2. Complete the task (existing behavior)
3. Commit: `[milestone/<name>] task: <summary> #signal`

### /verify pass
1. Merge worktree to milestone branch
2. Worktree auto-cleaned by Claude Code

### Milestone completion
1. All slices verified
2. Commit: `[milestone/<name>] completion: <summary>`
3. `git checkout main && git merge milestone/<name>`
4. `git branch -d milestone/<name>`

## Crash Recovery

Git + atomic save combine for resilient recovery:

1. Session crashes mid-task
2. New session: `/start`
3. Git shows which branch we're on → which milestone is active
4. Task files in `tasks/active/` show exact resume point (Progress section)
5. `git status` shows any uncommitted changes → what was in-flight
6. `git log` shows last commit → last successful `/save`
7. Resume from the last clean state

If a worktree was in-flight:
- Worktree still exists on disk with partial changes
- Orchestrator can inspect it, decide to continue or discard
- No damage to milestone branch — worktree is isolated

## What This Does NOT Cover

- **Force pushes** — never. Main is protected by convention.
- **Rebasing** — not needed for markdown repos. Merge commits are fine.
- **Multiple users** — this spec is single-user. Multi-user coordination is a future concern.

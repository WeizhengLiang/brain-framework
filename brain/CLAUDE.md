# Brain — Workspace Reference

> **Canonical source for all system specs: root `CLAUDE.md`**
> This file contains only brain-specific orientation and pointers. Never restate what root defines — reference it.

## Structure
- `inbox/` — raw capture, unsorted
- `tasks/` — backlog.md, active.md (index), active/ (task files), done.md
- `workstation/_roles/` — planner, implementer, reviewer, generalist
- `workstation/projects/` — one folder per project
- `skills/` — capability modules loaded per-task, with trigger-based auto-matching
- `wiki/` — persistent knowledge: concepts, references, decisions, retrospectives, playbook
- `templates/` — milestone, slice, task, skill, decision, concept, retrospective

## What's Defined Where

| Concept | Canonical Location |
|---------|-------------------|
| Phase model (Research→Plan→Execute→Completion) | Root CLAUDE.md → "Phases per Level" |
| Task hierarchy (Milestone/Slice/Task) | Root CLAUDE.md → "Task Hierarchy" |
| 50% context rule | Root CLAUDE.md → "The 50% Context Rule" |
| Agent team & orchestration | Root CLAUDE.md → "Agent Team & Roles" |
| Skills system & auto-suggest | Root CLAUDE.md → "Skills System" |
| All commands (/start, /save, /done, etc.) | Root CLAUDE.md → "Commands" |
| All workflows (starting work, inbox, etc.) | Root CLAUDE.md → "Workflows" |
| Self-learning system (3 tiers) | Root CLAUDE.md → "Self-Learning System" |
| Atomic save spec | Root CLAUDE.md → "/save" |
| Core rules | Root CLAUDE.md → "Core Rules" |
| Conventions (dates, filenames, tags) | Root CLAUDE.md → "Conventions" |
| Role definitions | `workstation/_roles/*/CLAUDE.md` |
| Skill definitions | `skills/*.md` |

## Brain-Specific Notes

- `wiki/playbook.md` is required reading at every session start — it contains operational rules earned from experience
- Active tasks are individual files in `tasks/active/` with live progress state — this is the resume mechanism after a crash or session end
- When spawning subagents, the Orchestrator loads: role CLAUDE.md + task file + matched skills. Root CLAUDE.md is inherited automatically via directory hierarchy

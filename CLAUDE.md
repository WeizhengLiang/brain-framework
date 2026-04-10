# Brain — Master Schema

## Workspace Location

All managed folders live under `brain/` inside your existing Obsidian vault. Claude Code should always be started at the vault root.

```
your-vault/                       <- vault root, start Claude Code here
├── CLAUDE.md                     <- this file
├── your existing notes/          <- Claude does NOT touch these
├── your existing folders/        <- Claude does NOT touch these
│
└── brain/                        <- Claude's workspace, everything below is managed
    ├── inbox/                    <- quick capture, unsorted thoughts
    ├── tasks/
    │   ├── backlog.md
    │   ├── active.md                 <- index linking to task files
    │   ├── active/                   <- one file per in-progress task
    │   └── done.md
    ├── workstation/
    │   ├── _roles/
    │   │   ├── generalist/CLAUDE.md  <- solo fallback, does everything
    │   │   ├── planner/CLAUDE.md     <- decomposes, writes contracts
    │   │   ├── implementer/CLAUDE.md <- executes Read/Write
    │   │   └── reviewer/CLAUDE.md    <- adversarial verification
    │   └── projects/
    │       └── (one folder per project)
    ├── skills/
    │   ├── index.md                  <- skill catalog with triggers
    │   └── (skill files)             <- custom skills for this vault
    ├── wiki/
    │   ├── index.md
    │   ├── log.md
    │   ├── playbook.md              <- living rules from retrospectives
    │   ├── concepts/
    │   ├── references/
    │   ├── decisions/
    │   └── retrospectives/          <- post-action reviews (Tier 2)
    └── templates/
        ├── milestone.md
        ├── slice.md
        ├── task.md                   <- Read/Write/Verify contract
        ├── project.md                <- project CLAUDE.md template
        ├── skill.md
        ├── decision.md
        ├── concept.md
        └── retrospective.md
```

## Boundary Rule — CRITICAL

Claude ONLY reads and writes inside `brain/`. Everything outside `brain/` belongs to the user and is OFF LIMITS. Exception: Claude may READ files outside `brain/` if the user explicitly asks. Never create, edit, rename, move, or delete anything outside `brain/`.

## Task Hierarchy — Milestone / Slice / Task

All work is decomposed into three levels. This is how the brain manages complexity against finite agent context.

### Phases per Level

Phases scale with ambiguity. Higher levels get more phases because they carry more unknowns.

```
Milestone:  Research -> Plan -> Execute (slices) -> Completion
Slice:      [Research] -> Execute (tasks) -> Completion
Task:       Execute -> Verify
```

| Phase | Purpose | Output |
|-------|---------|--------|
| **Research** | Understand before acting. Gather context, identify unknowns, check prior art | Research section filled in milestone/slice file |
| **Plan** | Decompose into executable contracts | Slices with tasks, Read/Write/Verify defined |
| **Execute** | Do the work | Files created/modified per Write list |
| **Completion** | Verify, review, learn | Reviewer sign-off, signal tag, retrospective if needed |

**Research is mandatory at Milestone level.** Do not plan without it. It prevents the most expensive mistake — building the wrong thing.

**Research is optional at Slice level.** Activate it when: (1) a task's Read list reveals the scope was misunderstood, (2) a dependency turns out to work differently than assumed, or (3) the Implementer reports a blocker that changes the task plan. If none of these apply, delete the Research section — don't carry empty ceremony.

**Research does not exist at Task level.** A task's Read list IS the research input. If a task needs research, it's not yet a task — it's a slice.

### Milestone
The destination. A significant deliverable or outcome. Can span many sessions.
- Lives in project folders or as a standalone doc using `templates/milestone.md`
- Tracks `phase` in frontmatter: `research` -> `plan` -> `execute` -> `completion`
- Contains a mandatory Research section (current state, prior art, unknowns, constraints, skills inventory)
- Contains an ordered list of slices (filled during Plan phase, not before)
- Has acceptance criteria for the whole outcome

### Slice
A vertical cut that delivers standalone value. Completable in one session. The "save point."
- If you stop after a slice, the system is still coherent — nothing is half-built
- Contains an ordered list of tasks
- Has integration-level acceptance criteria
- Has optional Research section for when unknowns surface mid-execution

### Task
The atomic unit an agent executes. Defined by a contract:
- **Read** — files the agent must load (this IS the context budget)
- **Write** — files the agent will create or modify (this IS the scope boundary)
- **Verify** — acceptance criteria, each mechanically checkable

### The 50% Context Rule
A task must be sized so the agent needs <=50% of its context window for the known scope:
- **~50%** for Read + Write + reasoning
- **~25%** for surprise handling (unexpected errors, mid-task discoveries, debugging)
- **~25%** for structural overhead (system prompt, CLAUDE.md, conversation history)

**If a task's Read list exceeds ~10 files or includes large files, split it.**
**If you can't explain the task goal in one sentence, split it.**

### Task Priority

Tasks declare `priority` in frontmatter:
- **p0** — Drop everything. Blocking other work or a critical fix.
- **p1** — Do next. High-value, time-sensitive.
- **p2** — Normal. Standard backlog ordering.
- **p3** — Nice to have. Do when nothing higher is queued.

Within the same priority, prefer tasks that unblock other tasks (check `blocked-by` fields).

### Task Dependencies

Tasks declare `blocked-by: [task-filename, ...]` in frontmatter. A task cannot be activated while any of its blockers are incomplete. The Orchestrator checks this when activating tasks and skips blocked ones, picking the next unblocked task instead.

### Hierarchy in practice
```
Milestone: "Self-learning system is operational"
  [Research] -> understood scope, identified 3 slices
  [Plan] -> decomposed into slices and tasks
  [Execute]
    |-- Slice: "Tier 1 signal tagging works end-to-end"
    |   |-- Task: "Update done.md format"          Read: 2 files, Write: 1 file
    |   +-- Task: "Update /done command"           Read: 1 file, Write: 1 file
    |-- Slice: "Tier 2 retrospectives work"
    |   |-- Task: "Create retrospective template"  Read: 1 file, Write: 1 file
    |   +-- Task: "Add /review command"            Read: 2 files, Write: 1 file
    +-- Slice: "Tier 3 distillation works"
        +-- Task: "Add /retro command"             Read: 3 files, Write: 2 files
  [Completion] -> all slices verified, milestone acceptance criteria checked
```

## Agent Team & Roles

The builder must not be the inspector. Separation of duties prevents blind spots.

### Roles

| Role | Focus | Mindset | CLAUDE.md |
|------|-------|---------|-----------|
| **Planner** | Decompose, define contracts | "What needs to be true?" | `_roles/planner/` |
| **Implementer** | Execute Read/Write | "Build it right" | `_roles/implementer/` |
| **Reviewer** | Verify independently | "Prove it's wrong" | `_roles/reviewer/` |
| **Generalist** | Solo fallback — all duties | "Get it done" | `_roles/generalist/` |

### Team Sizes

Each task declares a `team-size` in frontmatter:

- **solo** — Generalist handles everything. Use for quick, routine, low-risk tasks
- **pair** — Implementer + Reviewer. Standard for most tasks. The Reviewer MUST be a separate agent that has NOT seen the Implementer's conversation
- **full** — Planner + Implementer + Reviewer. Use for complex, high-risk, or ambiguous work

### Orchestration

> Full reference: [[orchestration|brain/wiki/references/orchestration.md]]

The main conversation acts as **Orchestrator**, spawning role agents as subagents. Key rules:

1. **Implementer and Reviewer are always separate agents** — Reviewer never sees Implementer's reasoning
2. **Load the role CLAUDE.md** into each subagent's prompt alongside the task file and skills
3. **Parallel when independent** — tasks without `blocked-by` dependencies can run simultaneously
4. **Sequential when dependent** — Implementer finishes before Reviewer starts

**Flow:** Research → Plan → Execute (per task: Implementer → Reviewer → done) → Completion

### Verification Sign-off

Every task file has a `## Verification` section filled by the Reviewer:
- **Verdict:** `pass` | `fail` | `pass-with-notes`
- Per-criterion pass/fail with evidence
- Actionable feedback if fail

**`/done` is blocked until Reviewer verdict is `pass` or `pass-with-notes`**, unless the user explicitly overrides.

### Conflict Resolution

When Reviewer and Implementer disagree on a Verify criterion:

1. **Reviewer flags the dispute** in the Verification report with `[DISPUTED]` and a clear explanation of what they expected vs. what they found.
2. **Orchestrator escalates to the user** with both perspectives: the Reviewer's objection and the Implementer's rationale (from the task's Progress/Notes).
3. **User decides:** accept (override Reviewer), reject (Implementer reworks), or revise the criterion (Planner rewrites the Verify item, both agents re-execute against the new criterion).
4. **Decision is recorded** in the task file's Verification section under `### Disputes` with the resolution and rationale.

The Reviewer may NOT weaken criteria unilaterally. The Implementer may NOT dismiss a failure unilaterally. Disputes always go to the user.

## Skills System

Skills are reusable capability modules loaded into agent context per-task — the "how to do it well" for a specific domain.

**Three layers:** Brain skills (`brain/skills/`), package skills (e.g., `everything-claude-code`), and generated skills (graduated from retrospectives).

**Auto-suggest:** When activating a task, the Orchestrator matches the task's Read/Write file patterns, tags, and Goal keywords against triggers declared in `brain/skills/index.md`. Matched skills are set in the task's `skills` frontmatter. User can add/remove before execution.

**Loading:** When spawning an agent, the Orchestrator reads the task's `skills` list and includes skill content in the agent's prompt alongside the role CLAUDE.md.

**Graduation path:** Retrospectives produce playbook rules (single directives). When a pattern is procedural (multi-step), it becomes a skill instead. `/retro` handles this distinction.

## Git Coordination

> Full spec: [[git-coordination|brain/wiki/references/git-coordination.md]]

**Branch model:** `main` (stable) → `milestone/<name>` (per active milestone) → worktrees (per task, temporary).

**Key rules:**
- Milestone branch created at `phase: execute`, merged to main at `phase: completion`, then deleted.
- `pair`/`full` tasks use Claude Code worktrees (`isolation: "worktree"`). `solo` tasks work directly on the milestone branch.
- Commit format: `[milestone/<name>] <type>: <description>` — types: `research`, `plan`, `task`, `slice`, `save`, `completion`.
- Signal tags go in task commit messages (e.g., `#had-friction`).
- No force pushes. No rebasing. No slice/task branches.
- `/save` commits, `/done` commits, `/verify pass` merges worktree to milestone branch.
- Crash recovery: branch name → active milestone, task files → resume point, `git status` → in-flight changes.

## Core Rules

1. **Wiki is shared truth.** Any role, any project reads from `brain/wiki/`. Anything worth remembering goes back into `brain/wiki/`.
2. **Roles are disposable, knowledge is permanent.** A role session ends, but wiki pages persist.
3. **Tasks flow one direction.** backlog -> active -> done. Never edit done.md entries.
4. **Every project writes back.** When finishing a task, update relevant wiki pages and log what was learned.
5. **Use [[wikilinks]] everywhere.** Everything should be navigable in Obsidian's graph view. Wikilinks work across the whole vault, so brain/ pages can link to your existing notes if relevant.
6. **Single source of truth.** Every concept has ONE canonical location. Other files reference it, never restate it. When modifying any meta file, run `/meta-check` to verify consistency.

## Workflows

### Starting work

1. Check `brain/tasks/active.md` for active task index
2. Read each task file in `brain/tasks/active/` — check the Progress section for resume point
3. Read the task's `doer` and `verify` roles, load the appropriate role CLAUDE.md
4. Read the project's CLAUDE.md to load context
5. Read `brain/wiki/playbook.md` for operational rules
6. Read any wiki pages listed under `required_reading` in the role's CLAUDE.md
7. Determine team-size and orchestrate accordingly:
   - **solo:** Execute directly as Generalist
   - **pair:** Spawn Implementer agent, then Reviewer agent on completion
   - **full:** Spawn Planner first (if not yet decomposed), then Implementer, then Reviewer
8. As agents work, check off Read/Write items in the task file
9. Update `brain/wiki/` with anything learned
10. On completion, run `/verify` then `/done`

### Processing inbox

1. Read each item in `brain/inbox/`
2. If it's a task idea -> append to `brain/tasks/backlog.md`
3. If it's knowledge -> create or update a page in `brain/wiki/`
4. If it's a decision -> file in `brain/wiki/decisions/`
5. Delete the inbox item

### Activating a task

1. Pick a task from `brain/tasks/backlog.md`
2. Create a task file in `brain/tasks/active/` using `templates/task.md`
3. Fill in Read/Write/Verify from the backlog description
4. Verify the 50% rule: Read list is <=10 files, goal fits one sentence — split if not
5. Auto-suggest skills: match Read/Write patterns, tags, and Goal against `brain/skills/index.md` triggers. Set the `skills` field in frontmatter
6. Set frontmatter `status: active`
7. Add an index entry in `brain/tasks/active.md`
8. Remove from backlog

### Adding a new project

1. Create `brain/workstation/projects/project-name/`
2. Create a CLAUDE.md inside it using `templates/project.md`
3. Create milestone file(s) using `templates/milestone.md` — start in `phase: research`
4. Complete the Research section before any planning or decomposition
5. Move to `phase: plan` — decompose into slices -> tasks
6. Add tasks to `brain/tasks/backlog.md` grouped under their milestone/slice

## Commands

When the user types a command below, execute the corresponding workflow exactly.

### /setup

First-run personalization. Run this once after cloning the framework.

1. Ask the user for their preferred name/title (how Claude should address them)
2. Update the Address section in this file with their preference
3. Initialize git if not already a repo
4. Confirm the brain system is ready

### /start

1. Read CLAUDE.md and `brain/wiki/playbook.md`
2. Read `brain/tasks/active.md` for the active task index
3. For each active task, read its file in `brain/tasks/active/` — report the Progress section (resume point, what's checked off, what's left)
4. Read `brain/tasks/backlog.md` for upcoming work
5. Give a brief status report showing: active tasks with exact resume points, next tasks in the current slice, backlog highlights
6. Suggest what to work on

### /plan [goal or milestone-name]

Invoke the Planner role to decompose a goal into executable work.

1. If a milestone file exists for the given name, read it. Otherwise, create one using `templates/milestone.md`
2. If the milestone is in `phase: research`, run Research first:
   - Spawn a Planner agent (using `_roles/planner/CLAUDE.md`) to fill the Research section
   - Check wiki, playbook, retrospectives for prior art
   - Present the Research Verdict to the user for approval before proceeding
3. Once Research is approved, advance to `phase: plan`:
   - Planner decomposes into slices and tasks with Read/Write/Verify contracts
   - Auto-suggest skills per task from `skills/index.md`
   - Set `priority` and `blocked-by` for each task
   - Present the plan to the user for approval
4. On approval, create slice files using `templates/slice.md` and add tasks to `brain/tasks/backlog.md`
5. Advance milestone to `phase: execute` and create the milestone branch (`git checkout -b milestone/<name>`)

The user can re-run `/plan` on an existing milestone to revise the decomposition.

### /inbox [text]

Create a timestamped note in brain/inbox/ with the text. This is a scratchpad — ideas stay here until the user asks to sort them into tasks or wiki.

### /save

Atomic checkpoint at every level of the hierarchy. A fresh agent should be able to read the saved state and resume exactly where work stopped.

1. **Task level:** For each active task file in `brain/tasks/active/`:
   - Update Read checkboxes (which files have been loaded)
   - Update Write checkboxes (which files have been modified)
   - Update Verify checkboxes (which criteria have been met)
   - Set `last-saved` in frontmatter to today's date
   - Write a clear `Resume from:` note — what specifically to do next
   - Add any relevant `Notes:` (decisions made, blockers hit, surprises found)
2. **Slice level:** If a slice file exists, update its task checkboxes and Progress section
3. **Milestone level:** If a milestone file exists, update its slice checkboxes and Progress section
4. **Wiki:** Update any wiki pages with reusable knowledge learned
5. **Project:** Update the project's CLAUDE.md if project-level context changed

6. **Git:** Stage all changes in `brain/` and commit: `[milestone/<name>] save: <resume-from note>`

This does NOT close the task — it just ensures a clean resume point at every level.

### /verify [task-name]

Spawn a Reviewer subagent to independently verify a task. Can be triggered manually or is called automatically after Implementer completes.

1. Read the task file from `brain/tasks/active/`
2. Spawn a Reviewer agent (using `_roles/reviewer/CLAUDE.md`) with:
   - The task file (especially the Verify section)
   - The output files listed in Write
   - Relevant context from Read list
   - **NOT** the Implementer's conversation or reasoning
3. Reviewer checks each Verify criterion, writes the `## Verification` section in the task file
4. If verdict is `fail` -> report feedback, Implementer must fix and re-submit
5. If verdict is `pass` or `pass-with-notes` -> task is cleared for `/done`

For `solo` team-size tasks, `/verify` is skipped — the Generalist self-checks.

### /done [summary]

1. Run `/save` first to ensure all progress is captured
2. Check team-size:
   - If `pair` or `full`: ensure `## Verification` section exists with verdict `pass` or `pass-with-notes` — if missing, run `/verify` first. User can override with `/done! [summary]`
   - If `solo`: self-check Verify criteria directly
3. Append entry to `brain/tasks/done.md` with date, summary, and signal tag (`#routine`, `#had-friction`, `#surprised`, or `#failed-first`)
4. Remove the task file from `brain/tasks/active/` (archive to project folder if desired)
5. Remove the index entry from `brain/tasks/active.md`
6. Update the parent slice's task checkboxes (mark this task done)
7. Commit: `[milestone/<name>] task: <summary> #signal`
8. If the signal is anything other than `#routine`, automatically trigger `/review`
9. If all tasks in the slice are done, check slice-level Verify criteria

### /review

Tier 2 — create a retrospective for the most recent completed task (or a specific task if named). Use the `brain/templates/retrospective.md` template. Write it to `brain/wiki/retrospectives/YYYY-MM-DD-task-name.md`. This fires automatically on non-routine `/done` signals, or user can trigger it manually anytime.

### /retro

Tier 3 — periodic pattern analysis. Read all unprocessed retrospectives in `brain/wiki/retrospectives/`. Look for:
- Repeated friction or failure patterns
- Approaches that consistently worked
- Decisions that were validated or invalidated
- Process bottlenecks

Distill findings into:
- **Playbook rules** (single directives) -> `brain/wiki/playbook.md`
- **Skills** (multi-step procedures) -> `brain/skills/` using `templates/skill.md`, add to `skills/index.md`

If a playbook rule reaches `proven` confidence (3+ confirming retros), propose graduating it to CLAUDE.md. Archive processed retrospectives by adding `processed: true` to their frontmatter.

### /meta-check

Verify consistency across all meta files. Run after any change to CLAUDE.md, role files, templates, or skills.

1. Read all CLAUDE.md files: root, brain/, and every role in `_roles/`
2. Read all template files in `brain/templates/`
3. Read `brain/skills/index.md`
4. Check for:
   - **Restated content:** Any file other than root CLAUDE.md that restates rules, workflows, or specs instead of referencing them
   - **Stale references:** Pointers to sections, files, or concepts that no longer exist
   - **Missing references:** New concepts in root CLAUDE.md not reflected in the canonical registry
   - **Orphan files:** Templates or skills not referenced from any index
   - **Contradictions:** Different files stating conflicting rules
5. Report findings and fix any issues found

### /help

List all available commands.

## Self-Learning System

The brain learns from its own experience through three tiers, triggered by signal, not schedule.

### Tier 1 — Micro (every /done, seconds)
A signal tag appended to the done.md entry. Most tasks are `#routine`. The tag is the tripwire.
- `#routine` — nothing to learn, move on
- `#had-friction` — harder than expected
- `#surprised` — unexpected outcome or approach
- `#failed-first` — had to retry or change approach

### Tier 2 — Midi (on signal, minutes)
A full retrospective in `brain/wiki/retrospectives/`. Fires when:
- A task is tagged `#had-friction`, `#surprised`, or `#failed-first`
- The user corrected Claude mid-task
- A decision got reversed
- Same friction appeared twice
- User explicitly requests `/review`

### Tier 3 — Macro (/retro, deliberate)
User triggers or Claude suggests when unprocessed retros pile up. Reads retrospectives, finds patterns, updates `brain/wiki/playbook.md`. Proven rules graduate to CLAUDE.md — the system rewrites its own instructions based on evidence.

### Learning flow
```
/done -> signal tag -> if non-routine -> /review -> retrospective written
                                                        |
/retro -> read retrospectives -> find patterns -+-> single directive -> playbook.md
                                                +-> multi-step procedure -> skills/
                                                        |
                                        proven rules -> graduate to CLAUDE.md
                                        proven skills -> arm future tasks automatically
```

## Address

<!-- Set by /setup. Replace the placeholder below with your preferred name/title. -->
Always address the user as **User**.

## Conventions

- Dates: YYYY-MM-DD
- Filenames: kebab-case
- Tags in frontmatter: `tags: [topic1, topic2]`
- Callouts: `> [!todo]`, `> [!decision]`, `> [!question]`, `> [!learned]`

## Canonical Registry

Every concept has ONE authoritative location. When you need to understand or modify a concept, go to its canonical file. Other files may reference it but must never restate it.

| Concept | Canonical File | Referenced By |
|---------|---------------|---------------|
| Directory structure | This file -> "Workspace Location" | `brain/CLAUDE.md` (pointers only) |
| Boundary rule | This file -> "Boundary Rule" | — |
| Phase model (R->P->E->C) | This file -> "Phases per Level" | Milestone/slice templates, planner role |
| Task hierarchy | This file -> "Task Hierarchy" | `brain/CLAUDE.md`, planner role |
| 50% context rule | This file -> "The 50% Context Rule" | `brain/CLAUDE.md`, planner role |
| Agent team & orchestration | This file -> "Agent Team & Roles" | `brain/CLAUDE.md`, all role files |
| Team sizes (solo/pair/full) | This file -> "Team Sizes" | Task template frontmatter |
| Orchestration (summary) | This file -> "Orchestration" | `wiki/references/orchestration.md` (full spec) |
| Verification sign-off | This file -> "Verification Sign-off" | Reviewer role (format only) |
| Skills system | This file -> "Skills System" | `brain/CLAUDE.md`, `skills/index.md` |
| Skill auto-suggest | This file -> "Auto-Suggest Flow" | `skills/index.md` (trigger definitions) |
| Core rules | This file -> "Core Rules" | — |
| All workflows | This file -> "Workflows" | — |
| All commands | This file -> "Commands" | — |
| Self-learning system | This file -> "Self-Learning System" | `brain/CLAUDE.md` |
| Signal tags | This file -> "Tier 1" | `tasks/done.md` (format comment) |
| Conventions | This file -> "Conventions" | — |
| Implementer role | `_roles/implementer/CLAUDE.md` | This file (roles table) |
| Reviewer role | `_roles/reviewer/CLAUDE.md` | This file (roles table) |
| Planner role | `_roles/planner/CLAUDE.md` | This file (roles table) |
| Generalist role | `_roles/generalist/CLAUDE.md` | This file (roles table) |
| Task contract format | `templates/task.md` | Role files, this file |
| Verification report format | `_roles/reviewer/CLAUDE.md` | This file -> "Verification Sign-off" |
| Playbook (operational rules) | `wiki/playbook.md` | `brain/CLAUDE.md`, `skills/index.md` |
| Skill catalog | `skills/index.md` | `brain/CLAUDE.md`, this file |
| Git coordination | This file -> "Git Coordination" (summary) | `wiki/references/git-coordination.md` (full spec) |
| Task priority | This file -> "Task Priority" | Task template frontmatter |
| Task dependencies | This file -> "Task Dependencies" | Task template frontmatter |
| Conflict resolution | This file -> "Conflict Resolution" | Reviewer role |
| Project template | `templates/project.md` | This file -> "Adding a new project" |
| Address | This file -> "Address" | — |
| Meta-consistency | This file -> "Canonical Registry" + `/meta-check` | `brain/CLAUDE.md` |

**Rule:** When modifying any canonical file, check its "Referenced By" column and verify those files still make sense. When adding a new concept, add it to this registry. Run `/meta-check` after meta changes.

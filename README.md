# Brain Framework

A structured system for managing knowledge work with AI agents. Designed for [Obsidian](https://obsidian.md) + [Claude Code](https://claude.ai/claude-code).

Brain gives Claude Code a workspace with task management, agent orchestration, persistent knowledge, and a self-learning feedback loop — so it can handle complex, multi-session projects instead of one-off requests.

## What It Does

- **Decomposes work** into Milestones, Slices, and Tasks with clear contracts (Read/Write/Verify)
- **Orchestrates agent teams** — Planner, Implementer, and Reviewer roles with separation of duties
- **Manages context** — the 50% rule ensures tasks fit within agent context windows
- **Saves and resumes** — atomic checkpoints at every level so work survives session crashes
- **Learns from experience** — signal-driven retrospectives that feed back into operational rules and skills
- **Arms agents with skills** — domain-specific capability modules loaded per-task via trigger matching

## Quickstart

1. Clone or copy this repo into your Obsidian vault (or any directory):
   ```bash
   git clone https://github.com/YOUR_USERNAME/brain-framework.git your-vault
   # or copy into an existing vault:
   cp -r brain-framework/brain /path/to/your-vault/brain
   cp brain-framework/CLAUDE.md /path/to/your-vault/CLAUDE.md
   ```

2. Open the vault root in Claude Code:
   ```bash
   cd your-vault
   claude
   ```

3. Run setup:
   ```
   /setup
   ```

4. Start working:
   ```
   /start
   ```

## Walkthrough: Your First Project

This walks you from setup through completing your first task.

### 1. Setup
```
> /setup
```
Claude asks your preferred name, configures the system. One-time.

### 2. Plan a project
```
> /plan redesign wiki concept pages
```
Claude spawns the Planner role, which:
- Creates a milestone file with a Research section (current state, unknowns, risks)
- Presents findings and asks for approval
- Decomposes into slices and tasks with Read/Write/Verify contracts
- Presents the plan and asks for approval
- Creates a `milestone/wiki-redesign` branch

### 3. Start working
```
> /start
```
Claude reads active tasks, reports resume points, and suggests what to work on next. It checks `blocked-by` dependencies and picks the highest-priority unblocked task.

### 4. Execute
Claude activates a task and orchestrates based on `team-size`:
- **solo** — does the work directly, self-checks
- **pair** — spawns an Implementer agent, then a separate Reviewer agent
- **full** — Planner first, then Implementer, then Reviewer

### 5. Save progress
```
> /save
```
Checkpoints progress at every level (task, slice, milestone) and commits to git. A crash after `/save` loses nothing.

### 6. Complete a task
```
> /done updated 3 concept pages to standard template
```
Claude picks a signal tag (`#routine`, `#had-friction`, `#surprised`, `#failed-first`), appends to done.md, commits, and auto-triggers a retrospective if non-routine.

### 7. Learn from experience
Non-routine tasks get retrospectives automatically. When enough pile up:
```
> /retro
```
Claude reads retrospectives, finds patterns, and distills them into playbook rules or skills that improve future work.

### See it in action
Check `brain/workstation/projects/example-blog-redesign/` for a complete worked example showing a milestone, slice, task, and verification report — all filled in.

## Core Concepts

### Task Hierarchy

All work decomposes into three levels:

```
Milestone  ->  the destination, can span sessions
  Slice    ->  vertical cut delivering standalone value, one session
    Task   ->  atomic unit with a Read/Write/Verify contract
```

### Phases

Phases scale with ambiguity:

```
Milestone:  Research -> Plan -> Execute -> Completion
Slice:      [Research] -> Execute -> Completion
Task:       Execute -> Verify
```

Research is **mandatory** at the Milestone level. You don't plan without understanding the problem first.

### The 50% Context Rule

A task must need <=50% of the agent's context window:
- ~50% for the work itself
- ~25% for surprise handling
- ~25% for system overhead

If a task's Read list exceeds ~10 files, split it. If the goal doesn't fit in one sentence, split it.

### Agent Team

| Role | Focus | Mindset |
|------|-------|---------|
| **Planner** | Decompose, define contracts | "What needs to be true?" |
| **Implementer** | Execute Read/Write | "Build it right" |
| **Reviewer** | Verify independently | "Prove it's wrong" |
| **Generalist** | Solo fallback | "Get it done" |

**Key principle:** The Implementer and Reviewer are always separate agents. The Reviewer never sees the Implementer's reasoning — fresh context, fresh eyes.

**Team sizes** per task:
- `solo` — Generalist does everything (routine, low-risk)
- `pair` — Implementer + Reviewer (standard)
- `full` — Planner + Implementer + Reviewer (complex, high-risk)

### Task Contract

Every task defines:
- **Read** — files the agent must load (context budget)
- **Write** — files the agent will modify (scope boundary)
- **Verify** — acceptance criteria (mechanically checkable)

Read/Write checkboxes track progress. A fresh agent reads the task file and knows exactly where to resume.

### Skills System

Skills are domain-specific capability modules loaded into agents per-task:
- Auto-suggested by matching file patterns, tags, and keywords against skill triggers
- Brain skills (custom) + package skills (e.g., `everything-claude-code`) + generated skills (from retrospectives)
- Agents are armed with exactly the capabilities each task requires

### Self-Learning System

Three tiers, triggered by signal, not schedule:

1. **Tier 1 (Micro)** — Every `/done` gets a signal tag: `#routine`, `#had-friction`, `#surprised`, `#failed-first`
2. **Tier 2 (Midi)** — Non-routine signals trigger a retrospective
3. **Tier 3 (Macro)** — `/retro` distills retrospectives into playbook rules or skills

```
/done -> signal -> /review -> retrospective
                                    |
/retro -> find patterns -> playbook rules / skills
                                    |
                     proven rules -> graduate to CLAUDE.md
```

The system rewrites its own instructions based on evidence.

## Commands

| Command | Description |
|---------|-------------|
| `/setup` | First-run personalization |
| `/start` | Status report with exact resume points |
| `/plan [goal]` | Invoke Planner to research and decompose a goal |
| `/inbox [text]` | Quick capture to inbox |
| `/save` | Atomic checkpoint at every level (includes git commit) |
| `/verify` | Spawn Reviewer to independently verify work |
| `/done [summary]` | Complete a task with signal tag |
| `/review` | Create a retrospective |
| `/retro` | Analyze retrospectives, update playbook/skills |
| `/meta-check` | Verify consistency across all meta files |
| `/help` | List all commands |

## Directory Structure

```
your-vault/
├── CLAUDE.md                        <- master schema
└── brain/
    ├── inbox/                       <- quick capture
    ├── tasks/
    │   ├── backlog.md, active.md, done.md
    │   └── active/                  <- one file per in-progress task
    ├── workstation/
    │   ├── _roles/                  <- planner, implementer, reviewer, generalist
    │   └── projects/                <- one folder per project
    ├── skills/                      <- capability modules with triggers
    ├── wiki/                        <- persistent knowledge base
    │   ├── playbook.md              <- operational rules from experience
    │   └── retrospectives/          <- post-action reviews
    └── templates/                   <- milestone, slice, task, skill, etc.
```

## Customization

### Adding Custom Roles
Create a new directory in `brain/workstation/_roles/your-role/` with a `CLAUDE.md` that defines the role's identity, duties, and boundaries.

### Adding Custom Skills
Create a skill file in `brain/skills/` using `brain/templates/skill.md`. Add triggers for auto-matching and register it in `brain/skills/index.md`.

### Adjusting Team Sizes
Change the default `team-size` in `brain/templates/task.md` or set per-task in frontmatter.

## Requirements

- [Claude Code](https://claude.ai/claude-code) (CLI, desktop app, or IDE extension)
- Optionally: [Obsidian](https://obsidian.md) for the best human interface (graph view, wikilinks, callouts)

## License

MIT

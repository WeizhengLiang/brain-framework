# Skill Catalog

Skills are reusable capability modules loaded into agent context per-task. They provide domain knowledge and procedures — the "how to do it well" that roles and wiki don't cover.

> For the full orchestration workflow (how skills are loaded and used), see root CLAUDE.md → "Skills System."
> This file is the **skill catalog and authoring guide** — how to define and organize skills.

## Trigger Authoring Guide
Skills declare `triggers` in frontmatter. The Orchestrator matches these against a task's Read/Write lists, tags, and Goal:
- **File pattern:** `*.tsx`, `*.sql`, `components/` — matches against task Write list
- **Tag:** `#api`, `#security`, `#tdd` — matches against task tags
- **Keyword:** `database`, `auth`, `migration` — matches against task Goal text

## Catalog

### Brain System
- [[task-decomposition]] — how to break work into Milestone/Slice/Task correctly | triggers: `#planning`, `#decomposition`
- [[wiki-writing]] — how to write good wiki pages that persist value | triggers: `wiki/`, `#documentation`
- [[retrospective-analysis]] — how to extract actionable patterns from retros and framework proposals | triggers: `#retro`, `retrospectives/`, `proposals/`, `/evolve`

### External (installed packages)
<!-- Reference skills from everything-claude-code or other packages here -->
<!-- These don't need files in this directory — they're loaded from the package -->
- `everything-claude-code:coding-standards` — triggers: `*.ts`, `*.js`, `*.tsx`
- `everything-claude-code:security-review` — triggers: `#security`, `auth`, `input validation`
- `everything-claude-code:frontend-patterns` — triggers: `*.tsx`, `*.css`, `components/`
- `everything-claude-code:backend-patterns` — triggers: `api/`, `server`, `*.route.*`
- `everything-claude-code:postgres-patterns` — triggers: `*.sql`, `migrations/`, `#database`
- `everything-claude-code:tdd-workflow` — triggers: `*.test.*`, `*.spec.*`, `#tdd`
- `everything-claude-code:golang-patterns` — triggers: `*.go`

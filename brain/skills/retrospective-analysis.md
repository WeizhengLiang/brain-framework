---
name: "retrospective-analysis"
description: "How to extract actionable patterns from retrospectives for playbook and skills"
source: brain
triggers: ["#retro", "retrospectives/", "playbook", "/retro"]
tags: [learning, brain-system]
---
# Skill: Retrospective Analysis

## When to Use
When running `/retro` (Tier 3 analysis) or when reviewing multiple retrospectives to find patterns. This skill is for the analysis phase, not the individual review writing phase.

## Key Principles
1. **Patterns beat anecdotes.** One occurrence is an incident. Two is a coincidence. Three is a pattern worth codifying.
2. **Root causes, not symptoms.** "Tests failed" is a symptom. "No test coverage requirement in task contracts" is a root cause.
3. **Rules must be actionable.** "Be more careful" is not a rule. "Run linter before handoff" is.
4. **Skills beat rules.** If a pattern is a procedure (multi-step), it's a skill. If it's a single directive, it's a playbook rule.
5. **Confidence is earned.** emerging (1-2 retros) → proven (3+) → graduated to CLAUDE.md. Don't skip levels.

## Procedures

### Running a Retro Analysis
1. Read all retrospectives in `wiki/retrospectives/` where `processed: true` is NOT set
2. Group by theme — what recurring categories emerge?
3. For each theme with 2+ occurrences:
   - Identify the root cause (not the symptom)
   - Draft a rule or skill
   - Set confidence based on occurrence count
4. Update `wiki/playbook.md` with new rules
5. If a pattern is procedural (multi-step), create a skill in `skills/` instead
6. Mark processed retros with `processed: true` in frontmatter
7. Check existing playbook rules — do any get upgraded from emerging → proven?
8. Check proven rules — should any graduate to CLAUDE.md?

### Graduating a Rule to CLAUDE.md
1. Rule must be at confidence `proven` (3+ confirming retros)
2. Rule must be general enough to apply across projects, not project-specific
3. Propose the change to Orchestrator — never self-modify CLAUDE.md without approval
4. If approved, add to the appropriate section of CLAUDE.md
5. Remove from playbook (it now lives at a higher level)

### Creating a Skill from Patterns
1. Pattern must be procedural — multiple steps, not a single directive
2. Create using `templates/skill.md`
3. Add triggers based on the contexts where the pattern appeared
4. Add to `skills/index.md`
5. Reference the source retrospectives

## Anti-Patterns
- **Premature graduation** — promoting a rule after one occurrence. Wait for evidence.
- **Symptom rules** — "don't let tests fail" instead of addressing why they failed.
- **Rule bloat** — 50 playbook rules nobody reads. Keep it lean. Merge overlapping rules.
- **Ignoring success patterns** — only analyzing failures. "What worked" is as valuable as "what broke."

## Checklist
- [ ] All unprocessed retros have been read
- [ ] Patterns grouped by theme
- [ ] Each new rule has a root cause, not just a symptom
- [ ] Confidence levels set correctly
- [ ] Existing rules checked for upgrades
- [ ] Processed retros marked
- [ ] Playbook updated
- [ ] Skills created for procedural patterns

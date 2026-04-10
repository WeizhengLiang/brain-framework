---
name: "wiki-writing"
description: "How to write wiki pages that persist value and stay discoverable"
source: brain
triggers: ["wiki/", "#documentation", "concepts/", "references/", "decisions/"]
tags: [documentation, brain-system]
---
# Skill: Wiki Writing

## When to Use
When creating or updating pages in `brain/wiki/`. Every role should load this skill when writing back to the wiki.

## Key Principles
1. **Write for the stranger.** The next agent reading this has zero context. No pronouns without antecedents, no "as discussed."
2. **One page, one concept.** If you're explaining two things, make two pages and link them.
3. **Wikilinks are the graph.** Every page should link to related pages. Orphan pages are invisible pages.
4. **Frontmatter is for machines.** Tags, dates, status — keep it structured for queries.
5. **Body is for humans.** Clear prose, callouts for important items, no wall-of-text.

## Procedures

### Creating a New Wiki Page
1. Check `wiki/index.md` — does this concept already exist? Update, don't duplicate.
2. Choose the right directory: `concepts/` for knowledge, `references/` for external pointers, `decisions/` for choices made.
3. Use the appropriate template from `templates/`
4. Add [[wikilinks]] to all related existing pages
5. Update `wiki/index.md` with a one-line entry
6. Update `wiki/log.md` with date and what was added/changed

### Updating an Existing Page
1. Read the current page fully before editing
2. Preserve existing links and structure
3. Add a `> [!learned]` callout if the update contains a new insight
4. Update `wiki/log.md` with date and what changed

## Anti-Patterns
- **Journal entries** — "today I learned X." Wiki is timeless knowledge, not a diary. State the fact.
- **Duplicate pages** — two pages explaining the same concept. Search first.
- **Link deserts** — pages with no wikilinks. Every page connects to something.
- **Stale pages** — information that was true once. If you find one, update or mark it with `> [!question] May be outdated`

## Checklist
- [ ] Page exists in the right directory (concepts/references/decisions)
- [ ] Frontmatter has tags
- [ ] At least 2 wikilinks to other pages
- [ ] Added to wiki/index.md
- [ ] Logged in wiki/log.md
- [ ] No "I" or "we" — write as timeless fact

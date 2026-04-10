---
status: backlog
milestone: "{{milestone}}"
slice: "{{slice}}"
priority: # p0 (drop everything) | p1 (do next) | p2 (normal) | p3 (nice to have)
blocked-by: [] # task filenames this task depends on — cannot start until all are done
context-load: # light | medium | heavy — must be light or medium (50% rule)
team-size: pair # solo | pair | full
doer: implementer
verify: reviewer # must differ from doer
skills: [] # auto-suggested or manually assigned — loaded into agent context
last-saved:
tags: []
---
# Task: {{title}}

## Goal
What does "done" look like? One sentence.

## Read
<!-- Files/pages the agent must load. This IS the context budget. -->
<!-- If this list exceeds ~10 files or includes large files, the task is too big — split it. -->
<!-- Check off items as they are loaded during execution. -->
- [ ]

## Write
<!-- Files/pages the agent will create or modify. This IS the scope boundary. -->
<!-- Anything not listed here is out of scope. Check off as modified. -->
- [ ]

## Verify
<!-- Acceptance criteria. Each must be mechanically checkable. -->
<!-- Owned by Planner/Commander. Implementer works against these. Reviewer checks these. -->
<!-- The task is not done until every box is checked AND Reviewer signs off. -->
- [ ]

## Progress
<!-- Updated by /save. A fresh agent reads this section to know exactly where to resume. -->
**Last saved:**
**Resume from:**
**Notes:**

## Verification
<!-- Filled by Reviewer after Implementer hands off. Do not pre-fill. -->

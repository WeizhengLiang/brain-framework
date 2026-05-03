---
date: {{date}}
status: proposed # proposed | approved | rejected | implemented
confidence: emerging # emerging (2+ retros) | proven (3+ retros)
target: "{{type/name}}" # template/<name> | command/<name> | role/<name> | orchestration | skill-system | CLAUDE.md/<section-name> | wiki/<page>
source-retros: [] # [[retro-1]], [[retro-2]]
tags: [framework-proposal]
---
# Proposal: {{title}}

## Problem
What is broken, missing, or inefficient in the target component? Reference the source retrospectives.

## Evidence
<!-- Summarize the pattern across retrospectives. What keeps happening? -->
- **Retro 1:** [[retro-link]] — one-line summary of the relevant finding
- **Retro 2:** [[retro-link]] — one-line summary of the relevant finding

## Proposed Change

### Before
<!-- Current state of the target component. Quote or describe the relevant section. -->

### After
<!-- Proposed new state. Show the concrete change. -->

## Impact Analysis
<!-- What else does this change affect? Check the Canonical Registry "Referenced By" column. -->
- **Files affected:**
- **Downstream effects:**

## Risk
<!-- What could go wrong if this change is applied? -->

## Decision
<!-- Filled when status changes from "proposed". -->
<!-- **Approved:** date, rationale -->
<!-- **Rejected:** date, rationale -->
<!-- **Implemented:** date, verified by /meta-check (pass/fail) -->

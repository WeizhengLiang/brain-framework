---
date: {{date}}
task: "{{task-link}}"
signal: # routine | had-friction | surprised | failed-first
scope: project # project | framework | both
processed: false # set to true by /retro after patterns are extracted
tags: []
---
# Retrospective: {{title}}

## What happened
Brief description of the task and outcome.

## What worked
What went well or was effective?

## What didn't
Where was there friction, failure, or surprise?

## Root cause
Why did the friction/failure happen? Dig past symptoms.

## Lesson
One concrete, actionable rule for next time.

## Applies to
Where should this lesson surface? (`playbook`, `CLAUDE.md`, role, project)

## Framework Impact
<!-- Include this section only when scope is "framework" or "both". Delete if scope is "project". -->

### Target Component
<!-- Which framework element is involved? Pick ONE primary target. -->
<!-- Valid targets: template/<name> | command/<name> | role/<name> | orchestration | skill-system | CLAUDE.md/<section-name> | wiki/<page> -->

### What's Wrong
<!-- Describe the specific defect, gap, or inefficiency in the framework component. -->

### Suggested Fix
<!-- What would improve it? Be concrete — reference the actual file/section. -->

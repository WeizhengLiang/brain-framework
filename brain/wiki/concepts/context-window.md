---
tags: [brain-system, agents]
---
# Context Window

## Definition
The total amount of text (tokens) an AI agent can hold in working memory during a single session. Everything the agent reads, writes, and reasons about must fit within this window. When it fills up, the agent loses access to earlier information.

## Key Points
- The brain framework's [[50% rule]] sizes tasks so agents use at most half their context window for known work, leaving room for surprises and system overhead
- A task's Read list is the primary context budget — more files means less room for reasoning
- Large files consume disproportionate context; prefer targeted reads over loading entire files
- Context is also consumed by CLAUDE.md, role definitions, skills, and conversation history

## Related
- [[signal-tags]] — signal tags are lightweight precisely because they avoid consuming context
- [[wikilinks]] — wikilinks let agents navigate to information on demand instead of loading everything upfront

---
name: agent-memory
description: Interactive memory system for Claude Code. Save and recall information across conversations. Triggers on "remember this", "memo", "recall", "what did I say about...". Stores memories as markdown files with an index (MEMORY.md). Supports categorized storage (decisions, tasks, settings, knowledge) with deduplication.
---

# Skill: agent-memory — Save and recall information across conversations

Responds to "remember this" / "recall" commands, storing and retrieving memories as structured markdown files.

## Trigger
- Save mode: "remember this", "make a note", "save this", "memo"
- Search mode: "recall", "what did I say about...", "do you remember...?"

## Save Mode

1. Extract information to save from user's message
2. Determine category:
   - Decisions → MEMORY.md section
   - Tasks → MEMORY.md "Tasks" section
   - Settings → MEMORY.md "Environment" or "Settings"
   - Domain knowledge → separate file in memory/ directory
3. Check for duplicates against existing memories
4. Update if duplicate found, add if new
5. Respond briefly: "Saved."

### Memory File Location
- Main index: `<project_memory_dir>/MEMORY.md`
- Detail files: `<project_memory_dir>/<topic>.md`

The project memory directory is typically at:
`~/.claude/projects/<project-hash>/memory/`

### Memory File Format
```markdown
---
name: topic-name
description: One-line description for relevance matching
type: user|feedback|project|reference
---

Content here.
```

## Search Mode

1. Extract keywords from query
2. Search MEMORY.md → memory/*.md in order
3. If found, respond with context
4. If not found, honestly reply "No record found for that topic"

## Rules
- NEVER save .env contents, API keys, passwords, or secrets
- Keep entries concise (1-2 lines). Avoid verbose descriptions
- If MEMORY.md approaches 200 lines, split details into separate files and link them
- MEMORY.md is an index — it should contain links and brief descriptions, not full content

---
name: daily-schedule
description: Generate today's schedule with prioritized 15-minute time blocks. Fetches events from Google Calendar (via MCP), loads pending tasks from memory, classifies by urgency, and produces an optimized daily plan. Triggers on "good morning", "today's schedule", "what's on today".
compatibility: Requires Google Calendar MCP server configured in Claude Code
---

# Skill: daily-schedule — Prioritized daily schedule from Google Calendar + tasks

Generates a 15-minute time-blocked daily plan by combining calendar events with pending tasks.

## Trigger
- "good morning", "morning"
- "today's schedule", "what's on today?", "schedule"

## Execution Steps

1. Fetch today's events from Google Calendar MCP
2. Load pending and in-progress tasks from memory (MEMORY.md or task file)
3. Classify priority:
   - Urgent + Important: Must do today (meetings, deadlines)
   - Important: Should progress (development tasks, preparation)
   - Routine: Daily habits (email check, news review)
   - Low priority: If time permits
4. Generate 15-minute time-blocked schedule:
   ```
   09:00-09:15  Email & message check
   09:15-10:00  [Important] Draft proposal
   10:00-10:30  [Urgent] Team meeting
   ...
   ```
5. Display the schedule to user

## Rules
- Calendar events are fixed — schedule tasks around them
- High cognitive load tasks (development, writing) go in the morning
- Routine and light tasks go in the afternoon
- Preserve break times (lunch, etc.)
- If no Google Calendar MCP is configured, generate schedule from tasks only

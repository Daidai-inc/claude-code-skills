# Claude Code Skills by Daidai

Reusable skills for [Claude Code](https://claude.ai/claude-code).

## Skills

| Skill | Description |
|-------|-------------|
| [diagram](skills/diagram/) | Generate high-quality diagrams as HTML+Tailwind CSS → PNG |
| [agent-memory](skills/agent-memory/) | Interactive memory: save and recall info across conversations |
| [daily-schedule](skills/daily-schedule/) | 15-min time-blocked daily schedule from Google Calendar + tasks |

## Install

```bash
# Install all skills
npx skills add daidai-tools/claude-code-skills

# Or install individually (after cloning)
npx skills add ./skills/diagram
```

## Requirements

- [Claude Code](https://claude.ai/claude-code) CLI
- **diagram**: Node.js + Playwright (`npm install playwright && npx playwright install chromium`)
- **daily-schedule**: Google Calendar MCP server configured

## License

MIT

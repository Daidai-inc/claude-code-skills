# Claude Code Skills by Daidai

Reusable skills for [Claude Code](https://claude.ai/claude-code) — battle-tested in production on real projects including international sports events management systems.

**16 skills. Free to use. Just add to your project.**

## Install

```bash
# Clone and copy a skill
git clone https://github.com/AINAGOC/claude-code-skills.git
cp -r claude-code-skills/skills/<skill-name> .claude/skills/
```

## Skills

### Design & Visualization

| Skill | Description | Trigger |
|-------|-------------|---------|
| [diagram](skills/diagram/) | Generate diagrams as HTML+Tailwind CSS → PNG (SNS/slide/blog sizes) | "図解して" "ダイアグラム" |
| [frontend-slides](skills/frontend-slides/) | Create animation-rich HTML presentations from scratch or PowerPoint | "スライド作って" "プレゼン" |

### Development & Quality

| Skill | Description | Trigger |
|-------|-------------|---------|
| [verify-loop](skills/verify-loop/) | Forced verification gate after every code change — no skipping allowed | "検証して" "ちゃんと動くか確認" |
| [investigate](skills/investigate/) | Systematic debugging and root cause analysis | "バグ調査" "原因調べて" |
| [careful](skills/careful/) | Safe mode for production operations — double-checks before destructive actions | "本番操作" "慎重に" |
| [python-development](skills/python-development/) | Modern Python 3.12+ with Django/FastAPI, async patterns, production standards | Python開発全般 |
| [computer-use](skills/computer-use/) | Browser and GUI automation via Claude Computer Use | "ブラウザ操作" "自動化" |

### Strategy & Planning

| Skill | Description | Trigger |
|-------|-------------|---------|
| [plan-ceo-review](skills/plan-ceo-review/) | CEO-perspective decision review — 5-angle analysis with approve/reject verdict | "CEOレビュー" "意思決定" |
| [plan-eng-review](skills/plan-eng-review/) | Engineering design review — architecture, scalability, trade-offs | "設計レビュー" "エンジニアリング視点" |
| [office-hours](skills/office-hours/) | Quick idea validation — brainstorm → go/stop decision in minutes | "ちょっと相談" "アイデアある" |
| [retro](skills/retro/) | Weekly retrospective — what worked, what didn't, what to change | "振り返り" "レトロ" |

### AI & Agents

| Skill | Description | Trigger |
|-------|-------------|---------|
| [agent-memory](skills/agent-memory/) | Persistent memory system — save and recall information across conversations | "覚えておいて" "思い出して" |
| [daily-schedule](skills/daily-schedule/) | 15-min time-blocked daily schedule from Google Calendar + tasks | "今日の予定" "おはよう" |
| [find-skills](skills/find-skills/) | Discover and install skills for any task | "スキル探して" "できる？" |
| [skill-creator](skills/skill-creator/) | Interactively create new skills from scratch | "スキル作って" |

### Security

| Skill | Description | Trigger |
|-------|-------------|---------|
| [cso](skills/cso/) | Security review — OWASP Top 10 + STRIDE threat modeling | "セキュリティ確認" "脅威モデル" |

## Usage

Add a skill to your Claude Code project:

```
.claude/
└── skills/
    └── diagram/
        └── SKILL.md   ← the skill lives here
```

Claude Code auto-loads `SKILL.md` files. Once added, just use natural language triggers.

## Why Daidai Skills?

- **Production-tested**: Built for real workloads (international sports events, iOS apps, automated content pipelines)
- **Quality-gated**: Every skill passes a 3-reviewer Agent Teams quality gate before release
- **Composable**: Skills chain together — e.g. `investigate` → `verify-loop` → `careful` for safe fixes
- **Bilingual**: Triggers work in Japanese and English

## Requirements

- [Claude Code](https://claude.ai/claude-code) CLI
- **diagram**: Node.js + Playwright (`npm install playwright && npx playwright install chromium`)
- **daily-schedule**: Google Calendar MCP server configured
- **computer-use**: tmux environment recommended

## License

MIT — free to use, modify, and redistribute.

---

Built by [Daidai](https://daidai-inc.com) · [X @daidai_ai](https://x.com/daidai_ai)

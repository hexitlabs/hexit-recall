# 🧠 HexIT Recall

**Long-term semantic memory for AI assistants.**

HexIT Recall gives your AI assistant persistent, searchable memory that runs 100% locally on your server. Your AI remembers conversations, learns preferences, and gets smarter over time — with zero data leaving your machine.

## Features

- **Semantic Search** — Find memories by meaning, not just keywords
- **Daily Memory Logs** — Automatic structured daily journals
- **Memory Decay** — Old memories fade naturally, keeping context fresh and relevant
- **Observation Indexing** — Extracts patterns and insights from daily interactions
- **Self-Improvement Loop** — AI analyzes its own mistakes and learns from corrections
- **100% Local** — Powered by Ollama + nomic-embed-text. No cloud, no data sharing, no API costs
- **Clawdbot Skill** — One command install, works out of the box

## Quick Start

### As a Clawdbot Skill (recommended)

```bash
clawdbot skills install hexit-recall
```

### Manual Install

```bash
# 1. Clone the repo
git clone https://github.com/hexitlabs/hexit-recall.git
cd hexit-recall

# 2. Run setup
./scripts/setup.sh

# 3. Follow the prompts
```

## What Gets Installed

| Component | Purpose |
|-----------|---------|
| Ollama | Local inference engine (free, open source) |
| nomic-embed-text | Embedding model for semantic search |
| Memory file structure | MEMORY.md, daily logs, observations |
| Memory decay | Automatic relevance scoring over time |
| Observation indexer | Extracts patterns from daily interactions |
| Self-improvement loop | Learns from corrections and mistakes |

## Architecture

```
Your Server (100% local)
├── Ollama (embedding engine)
│   └── nomic-embed-text (384-dim embeddings)
├── Memory Files
│   ├── MEMORY.md          — Long-term curated memories
│   ├── memory/
│   │   ├── YYYY-MM-DD.md  — Daily structured logs
│   │   ├── observations.json — Extracted patterns
│   │   ├── lessons.md     — Mistakes & learnings
│   │   ├── preferences.md — User preferences
│   │   └── learning-queue.md — Topics to explore
│   └── sessions/          — Conversation transcripts
└── Scripts
    ├── memory-decay.ts    — Relevance scoring
    ├── observations-indexer.js — Pattern extraction
    └── synthesize.ts      — Self-improvement synthesis
```

## Memory Lifecycle

1. **Capture** — Every conversation is logged to daily memory files
2. **Index** — Observations are extracted and indexed for semantic search
3. **Search** — When the AI needs context, it searches by meaning across all memories
4. **Decay** — Older, less-accessed memories gradually lose relevance weight
5. **Synthesize** — Weekly analysis identifies patterns, mistakes, and improvement opportunities
6. **Prune** — Stale sessions are cleaned up to keep the system fast

## Configuration

HexIT Recall adds these settings to your `clawdbot.json`:

```json
{
  "memorySearch": {
    "enabled": true,
    "sources": ["memory", "sessions"],
    "provider": "openai",
    "remote": {
      "baseUrl": "http://localhost:11434/v1/",
      "apiKey": "ollama"
    },
    "model": "nomic-embed-text"
  }
}
```

## Memory File Templates

HexIT Recall creates structured templates for consistent memory formatting:

### Daily Log (memory/YYYY-MM-DD.md)
```markdown
# YYYY-MM-DD

## Decisions
- [What was decided and why]

## Completions
- [What got done today]

## Corrections
- [Mistakes made, lessons learned]

## Active
- [What's in progress]

## Notes
- [Anything else worth remembering]
```

### Lessons (memory/lessons.md)
```markdown
# Lessons Learned

## ❌ Mistakes
- [Date] [What went wrong] — [What to do instead]

## ✅ Learnings
- [Date] [What worked well] — [Why it worked]
```

## Scripts

### Memory Decay
```bash
# Update decay scores (run weekly)
npx tsx scripts/memory-decay.ts update

# View current scores
npx tsx scripts/memory-decay.ts status
```

### Observation Indexer
```bash
# Index today's observations
node scripts/observations-indexer.js

# Re-index all
node scripts/observations-indexer.js --all
```

### Self-Improvement Synthesis
```bash
# Run weekly synthesis
npx tsx scripts/synthesize.ts
```

## Privacy

HexIT Recall is designed with privacy as a core principle:

- **All data stays on your server** — nothing is sent to external services
- **Ollama runs locally** — embeddings are computed on your machine
- **No telemetry** — we don't track usage or collect any data
- **You own everything** — all memory files are plain markdown, fully portable

## Requirements

- Node.js 18+
- 4GB+ RAM (for Ollama + nomic-embed-text)
- ~2GB disk space (Ollama + model)
- Linux (Ubuntu 22.04+ recommended) or macOS

## License

MIT — Use it however you want.

## Built by HexIT Labs 🔷

Part of the HexIT AI infrastructure suite. Also check out [Vigil](https://github.com/hexitlabs/vigil) — AI agent safety firewall.

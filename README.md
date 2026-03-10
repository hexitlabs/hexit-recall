# 🧠 HexIT Recall

**Long-term semantic memory for AI assistants.**

Give your AI assistant persistent, searchable memory that runs 100% on your own server. It remembers conversations, picks up on preferences, and improves over time. Nothing leaves your machine.

## Features

- **Semantic Search** - finds memories by meaning, not keywords
- **Daily Memory Logs** - structured daily journals, auto-generated
- **Memory Decay** - old memories fade naturally so context stays relevant
- **Observation Indexing** - pulls patterns and insights from daily interactions
- **Self-Improvement** - tracks its own mistakes and learns from corrections
- **100% Local** - runs on Ollama + nomic-embed-text. No cloud calls, no data sharing, no API costs
- **Clawdbot Skill** - one command install

## Quick Start

### As a Clawdbot Skill

```bash
clawdbot skills install hexit-recall
```

### Manual Install

```bash
git clone https://github.com/hexitlabs/hexit-recall.git
cd hexit-recall
./scripts/setup.sh
```

## What Gets Installed

| Component | Purpose |
|-----------|---------|
| Ollama | Local inference engine (free, open source) |
| nomic-embed-text | Embedding model for semantic search |
| Memory file structure | MEMORY.md, daily logs, observations |
| Memory decay | Relevance scoring over time |
| Observation indexer | Pattern extraction from daily interactions |
| Self-improvement loop | Learns from corrections and mistakes |

## Architecture

```
Your Server (100% local)
├── Ollama (embedding engine)
│   └── nomic-embed-text (384-dim embeddings)
├── Memory Files
│   ├── MEMORY.md            # curated long-term memories
│   ├── memory/
│   │   ├── YYYY-MM-DD.md    # daily structured logs
│   │   ├── observations.json # extracted patterns
│   │   ├── lessons.md       # mistakes and learnings
│   │   ├── preferences.md   # user preferences
│   │   └── learning-queue.md # topics to explore
│   └── sessions/            # conversation transcripts
└── Scripts
    ├── memory-decay.ts      # relevance scoring
    ├── observations-indexer.js # pattern extraction
    └── synthesize.ts        # self-improvement synthesis
```

## How It Works

1. **Capture** - conversations get logged to daily memory files
2. **Index** - observations are extracted and indexed for semantic search
3. **Search** - when context is needed, it searches by meaning across all memories
4. **Decay** - older, less-accessed memories gradually lose relevance weight
5. **Synthesize** - weekly analysis spots patterns, mistakes, and improvement areas
6. **Prune** - stale sessions get cleaned up to keep things fast

## Configuration

Add to your `clawdbot.json`:

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
- [Date] What went wrong -> What to do instead

## ✅ Learnings
- [Date] What worked well -> Why it worked
```

## Scripts

### Memory Decay
```bash
npx tsx scripts/memory-decay.ts update   # update decay scores
npx tsx scripts/memory-decay.ts status   # view current scores
```

### Observation Indexer
```bash
node scripts/observations-indexer.js       # index today
node scripts/observations-indexer.js --all # re-index everything
```

### Self-Improvement Synthesis
```bash
npx tsx scripts/synthesize.ts
```

## Privacy

- All data stays on your server. Nothing gets sent anywhere.
- Ollama runs locally. Embeddings are computed on your machine.
- No telemetry. No tracking.
- Everything is plain markdown files. Fully portable, copy them wherever you want.

## Requirements

- Node.js 18+
- 4GB+ RAM (for Ollama + nomic-embed-text)
- ~2GB disk (Ollama + model)
- Linux (Ubuntu 22.04+) or macOS

## License

MIT

## Built by HexIT Labs 🔷

Part of the HexIT AI tooling stack. See also: [Vigil](https://github.com/hexitlabs/vigil) (AI agent safety firewall).

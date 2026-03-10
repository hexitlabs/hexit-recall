# HexIT Recall — AI Memory System

## What It Does
HexIT Recall provides long-term semantic memory for your AI assistant. It runs 100% locally using Ollama embeddings — no data leaves your server.

## Setup
Run the setup script to install Ollama, pull the embedding model, and create the memory file structure:

```bash
cd ~/clawd
bash skills/hexit-recall/scripts/setup.sh
```

## Memory Structure

Your workspace will have:
- `MEMORY.md` — Long-term curated memories (key facts, preferences, decisions)
- `memory/YYYY-MM-DD.md` — Daily structured logs (decisions, completions, corrections)
- `memory/lessons.md` — Tracked mistakes and learnings
- `memory/preferences.md` — User preferences (auto-extracted)
- `memory/observations.json` — Pattern index from daily interactions
- `memory/learning-queue.md` — Topics for the AI to explore

## How Memory Works

### Automatic (built into Clawdbot)
- **Memory search** runs before answering questions about prior work, decisions, or preferences
- **Compaction memory flush** saves important context when conversations get long
- **Session transcripts** are searchable alongside memory files

### Maintenance Scripts (run periodically)

**Memory Decay** — keeps memories fresh by scoring relevance over time:
```bash
npx tsx scripts/memory-decay.ts update    # Update scores
npx tsx scripts/memory-decay.ts status    # View current scores
```
Recommended: Run twice weekly via cron.

**Observation Indexer** — extracts patterns from daily logs:
```bash
node scripts/observations-indexer.js       # Index today
node scripts/observations-indexer.js --all # Re-index everything
```
Recommended: Run nightly via cron.

**Self-Improvement Synthesis** — analyzes mistakes and generates improvement proposals:
```bash
npx tsx scripts/synthesize.ts
```
Recommended: Run weekly.

## Clawdbot Config

Add to your `clawdbot.json` under agent defaults:

```json
{
  "memorySearch": {
    "enabled": true,
    "sources": ["memory", "sessions"],
    "experimental": { "sessionMemory": true },
    "provider": "openai",
    "remote": {
      "baseUrl": "http://localhost:11434/v1/",
      "apiKey": "ollama"
    },
    "model": "nomic-embed-text"
  }
}
```

## Cron Integration

For automated maintenance, add these cron jobs:

```bash
# Memory decay (twice weekly, e.g., Wed + Sun)
0 22 * * 0,3  cd ~/clawd && npx tsx scripts/memory-decay.ts update

# Observation indexer (nightly)
0 22 * * *    cd ~/clawd && node scripts/observations-indexer.js

# Self-improvement synthesis (weekly, Sunday)
0 4 * * 0     cd ~/clawd && npx tsx scripts/synthesize.ts
```

Or use Clawdbot's built-in cron system for agent-managed scheduling.

## Daily Log Format

Each day, the AI should update `memory/YYYY-MM-DD.md` with:

```markdown
# YYYY-MM-DD

## Decisions
- [What was decided and why]

## Completions
- [What got done]

## Corrections
- [Mistakes, what to do differently]

## Active
- [Work in progress]

## Notes
- [Anything worth remembering]
```

## Privacy
- All embeddings computed locally via Ollama
- No external API calls for memory operations
- All data stored as plain markdown files on your server
- Fully portable — copy the files anywhere

## Requirements
- Node.js 18+
- 4GB+ RAM (for Ollama)
- ~2GB disk (Ollama + nomic-embed-text model)
- Linux or macOS

## Built by HexIT Labs 🔷

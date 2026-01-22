# Neuro-Engine Plugin

An AI-powered Unity game development plugin for [Claude Code](https://claude.ai/claude-code).

Build complete games from Game Design Documents with autonomous AI agents.

## What is This?

The Neuro-Engine Plugin enables Claude Code to:
- **Orchestrate** multiple AI agents working in parallel
- **Generate** 3D models, textures, and audio via Meshy.ai and ElevenLabs
- **Evaluate** gameplay using vision language models (Gemini)
- **Persist** progress across sessions

## Quick Start

### 1. Clone the Main Engine Repository

```bash
git clone --recurse-submodules https://github.com/bruno1308/unity-neuro-engine.git
cd unity-neuro-engine
```

### 2. Set Up API Keys

Copy `.env.example` to `.env` and add your keys:

```bash
MESHY_API_KEY=your_meshy_key        # 3D generation (meshy.ai)
ELEVENLABS_API_KEY=your_key         # Audio generation
GEMINI_API_KEY=your_key             # Video analysis
```

### 3. Open in Unity

Open the project in Unity 6+ with the Universal Render Pipeline.

### 4. Start Claude Code

```bash
cd unity-neuro-engine
claude
```

### 5. Create Your First Game

```
/neuro-engine:iteration create "My Breakout Clone"
```

Then edit `Assets/Iteration1/GDD.md` with your game design.

## Commands

| Command | Description |
|---------|-------------|
| `/neuro-engine:iteration` | Create, list, or check iteration status |
| `/neuro-engine:orchestrate` | Start autonomous game development |
| `/neuro-engine:evaluate` | Run quality evaluation tiers |
| `/neuro-engine:blocker` | Report a blocking issue |
| `/neuro-engine:status` | Show current progress |
| `/neuro-engine:validate` | Check prerequisites |

## Architecture

The plugin is part of the 7-layer Neuro-Engine architecture:

| Layer | Purpose | Plugin Component |
|-------|---------|------------------|
| L7 | Asset Generation | `meshy`, `elevenlabs` skills |
| L6 | Orchestration | `mayor` agent, commands |
| L5 | Evaluation | `evaluator` agent, `gemini-evaluate` |
| L4 | Persistence | `hooks-persist` skill |
| L3 | Interaction | `scene-polecat` agent |
| L2 | Observation | `unity-state` skill |
| L1 | Code-First | `script-polecat` agent |

See [Architecture Documentation](https://github.com/bruno1308/unity-neuro-engine/blob/main/Docs/Architecture.md) for details.

## Requirements

- **Unity 6+** with Universal Render Pipeline
- **Claude Code** CLI
- **Unity-MCP** server (included via package)
- **Python 3.10+** (for asset generation scripts)

## Related Repositories

| Repository | Description |
|------------|-------------|
| [unity-neuro-engine](https://github.com/bruno1308/unity-neuro-engine) | Main Unity project with engine runtime |
| [unity-neuro-engine-plugin](https://github.com/bruno1308/unity-neuro-engine-plugin) | This plugin (Claude Code integration) |

## Project Structure

```
neuro-engine/
├── agents/          # AI agent definitions
├── commands/        # Slash commands (/neuro-engine:*)
├── skills/          # Proactive capabilities
├── hooks/           # Event handlers
└── CLAUDE.md        # Instructions for Claude
```

## How It Works

1. **You** write a Game Design Document (GDD)
2. **Mayor agent** decomposes it into tasks
3. **Polecat agents** implement in parallel (scripts, scenes, assets)
4. **Evaluator** grades the result
5. **Game Fixer** addresses any issues
6. Repeat until quality bar is met

## License

MIT License - See [LICENSE](https://github.com/bruno1308/unity-neuro-engine/blob/main/LICENSE)

## Contributing

Contributions welcome! Please read the [Architecture docs](https://github.com/bruno1308/unity-neuro-engine/blob/main/Docs/Architecture.md) first.

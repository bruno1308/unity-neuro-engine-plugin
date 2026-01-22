# CLAUDE.md - Neuro-Engine Plugin Instructions

These instructions are for Claude Code when working with the Neuro-Engine.

## Orchestration Rules (MANDATORY)

**When implementing a GDD, you MUST follow these rules.**

### Core Principles

1. **Mayor Never Implements** - The mayor agent delegates to polecat agents. It NEVER uses MCP tools directly.
2. **3 MCP Call Limit** - If a task requires more than 3 MCP calls, spawn an agent instead.
3. **Parallel Execution** - Spawn independent agents simultaneously using multiple Task tool calls in one message.
4. **Mandatory Verification** - Eyes Polecat must verify all integrations before completion.
5. **No Self-Approval** - Work must flow through verification agents.

### Correct Pattern

```
# CORRECT: Use orchestration skill
/neuro-engine:orchestrate start Assets/Iteration1/GDD.md

# CORRECT: Manual Mayor pattern with parallel agents
Task 1: script-polecat → "Write PlayerController script"
Task 2: scene-polecat → "Set up main scene with camera"
Task 3: asset-polecat → "Generate player model"
[All run in parallel via single message with multiple Task calls]
```

### Violations

```
# WRONG: Direct MCP tool use when implementing GDD
mcp__unity-mcp__create_script (VIOLATION - should use script-polecat)
mcp__unity-mcp__manage_gameobject (VIOLATION - should use scene-polecat)
```

### Enforcement

- Pre-commit hooks warn on direct MCP use
- Agent frontmatter restricts available tools
- Verification checklist must pass

## Agent Reference

| Agent | Role | When to Use |
|-------|------|-------------|
| `mayor` | Orchestration | Decompose GDD, assign tasks |
| `script-polecat` | C# Code | Writing Unity scripts |
| `scene-polecat` | Scene Setup | Modifying scenes, prefabs, GameObjects |
| `asset-polecat` | Asset Gen | Creating 3D models, audio, textures |
| `eyes-polecat` | Observation | Checking game state, verification |
| `evaluator` | Quality | Grading against criteria |
| `game-tester` | Testing | Finding bugs via play |
| `game-fixer` | Bug Fixes | Addressing found issues |
| `code-reviewer-layers` | Review | Engine core changes |

## Skill Reference

| Skill | Trigger Condition |
|-------|-------------------|
| `validate` | Before development, after setup changes |
| `status` | Session start, progress questions |
| `unity-state` | Questions about scene state |
| `hooks-persist` | After completing tasks |
| `layer-review` | Modifying engine core |
| `meshy` | Creating 3D assets |
| `elevenlabs` | Creating audio |
| `gemini-evaluate` | Video analysis of gameplay |
| `extract-alpha` | Making sprites transparent |

## Layer Architecture

```
L7: Generative Assets    → meshy, elevenlabs, extract-alpha
L6: Agent Orchestration  → mayor, commands
L5: Evaluation           → evaluator, gemini-evaluate
L4: Persistence          → hooks-persist
L3: Interaction          → scene-polecat
L2: Observation          → unity-state, eyes-polecat
L1: Code-First           → script-polecat
```

## Hooks

| Event | Behavior |
|-------|----------|
| `SessionStart` | Show iteration status |
| `PreToolUse` (MCP) | Quick validation |
| `PostToolUse` (engine) | Layer review trigger |
| `PostToolUse` (iteration) | Auto-persist |

## Workflow

1. User creates iteration: `/neuro-engine:iteration create "Game Name"`
2. User edits GDD: `Assets/Iteration{N}/GDD.md`
3. Build via: `/neuro-engine:orchestrate start` OR manual agent spawning
4. Evaluate: `/neuro-engine:evaluate all`
5. Fix issues via `game-fixer` agent
6. Repeat until pass

## Key Files

| Path | Purpose |
|------|---------|
| `ORCHESTRATION_RULES.md` | Detailed orchestration rules |
| `ARCHITECTURE.md` | Plugin architecture |
| `agents/*.md` | Agent definitions |
| `skills/*/SKILL.md` | Skill procedures |
| `commands/*.md` | Command definitions |

## Error Handling

- **MCP Connection Lost**: Retry with backoff, then escalate
- **API Rate Limit**: Wait and retry per API guidance
- **Compilation Error**: Fix immediately, don't proceed with broken code
- **Evaluation Fail**: Use game-fixer agent to address issues

## Important Reminders

1. Always run `/neuro-engine:validate` before starting work
2. Use parallel Task calls for independent work
3. Verify integrations with eyes-polecat before declaring done
4. Persist progress with hooks-persist after milestones
5. Never approve your own work - verification agents must confirm

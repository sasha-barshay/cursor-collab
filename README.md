# Cursor Collab

**Multi-LLM Collaboration Protocol for Cursor IDE**

A lightweight protocol that enables multiple AI models (Claude, Gemini, GPT, etc.) to collaborate on the same task within Cursor IDE, building shared knowledge incrementally.

## The Problem

When using multiple AI models in Cursor, each operates in isolation. They can't see what other models discovered, leading to:
- Duplicated work
- Contradictory findings
- Lost context when switching models

## The Solution

A simple file-based protocol where:
- Models share a **Knowledge Hub** (`.khub` file)
- Each contribution is **attributed** and **verified**
- Knowledge **accumulates** across model switches
- Human can **moderate** and steer the collaboration

## Quick Start

### 1. Install

```bash
# Create the collab folder
mkdir -p ~/.cursor/collab

# Copy the protocol
cp Collaboration.md ~/.cursor/collab/
```

### 2. Configure Cursor

Go to **Cursor Settings → General → Rules for AI** and paste:

```
For LLM Collaboration Mode, follow the protocol at: ~/.cursor/collab/Collaboration.md

When user sends "." or "r" or "continue" or "round":
1. Find your .khub file by searching for your CURSOR_TRACE_ID in ~/.cursor/collab/*.khub
2. Read what other models contributed
3. Do useful work (verify, research, run commands)
4. Add your verified findings to the .khub file

When user sends other input:
1. First add user's input to the .khub file under ## User Input
2. Then respond and add your response to the file
3. This ensures other models see the user's input

Always prefix contributions with your model name (Opus, Gemini, GPT, Claude).
```

### 3. Start a Session

Create a `.khub` file for your topic:

```bash
cat > ~/.cursor/collab/MyProject.khub << 'EOF'
# session: <your-cursor-trace-id>
# topic: My Project Description

## Context
[Describe the project/task here]

## Status
→ Ready to start
EOF
```

### 4. Collaborate

- Type `.` or `r` in any Cursor chat to trigger a collaboration round
- Switch models and type `r` again - they'll see previous contributions
- Add your input - models will record it for others to see

## How It Works

```
┌─────────────────┐     ┌─────────────────┐     ┌─────────────────┐
│  Claude (Opus)  │     │     Gemini      │     │    You (Human)  │
└────────┬────────┘     └────────┬────────┘     └────────┬────────┘
         │                       │                       │
         │    type "r"           │    type "r"           │
         ▼                       ▼                       ▼
    ┌─────────────────────────────────────────────────────────┐
    │                    MyProject.khub                       │
    │  ─────────────────────────────────────────────────────  │
    │  ## Hardware Assessment                                 │
    │  Opus: Server has 8GB RAM - verified via `free -h`      │
    │  Gemini: CPU supports AVX2 - verified via /proc/cpuinfo │
    │                                                         │
    │  ## User Input                                          │
    │  User: What about GPU support?                          │
    │                                                         │
    │  ## Status                                              │
    │  Opus: ✓ RAM checked                                    │
    │  Gemini: → Checking GPU next                            │
    └─────────────────────────────────────────────────────────┘
```

## .khub File Format

```markdown
# session: <CURSOR_TRACE_ID>
# topic: Human-readable topic name

## Inbox
U1 [open]: <short summary of question/task>

## Context
[Project background, server specs, goals, etc.]

## [Topic Section]
ModelName: [Verified finding] - verified via [method]

## Open Questions
ModelName: ? [Question needing resolution]

## Status
ModelName: ✓ [Completed item]
ModelName: ⏳ [In progress]
ModelName: → [Next action / handoff]
```

## Keeping the KHub Thin

A `.khub` is a **knowledge hub**, not a transcript.

- Put new questions/tasks in `## Inbox` as **1-line summaries**
- When an item is resolved, **remove it from Inbox** and keep the durable outcome in the relevant section
- Optionally record a pointer in `## Resolved` (e.g., `U7 [✓]: Answer recorded in Networking`)

This keeps the file scannable and prevents it from growing without bound.

## Collaboration Rules

1. **Read first** - Understand what others have established
2. **Don't repeat** - Build on existing findings, don't duplicate
3. **Verify before adding** - No unconfirmed speculation
4. **One fact per line** - Keep entries atomic and scannable
5. **Fill gaps** - Look for missing info, unanswered questions
6. **Challenge if needed** - Note contradictions clearly

## Commands

| Command | Action |
|---------|--------|
| `.` or `r` | Trigger collaboration round |
| `/session Name` | Switch to different .khub |
| `/summarize` | Get summary of current state |
| `/ask @Model` | Direct question to specific model |
| `/next` | Suggest next action items |
| `/done` | End the collaboration session |

## Markers

| Marker | Meaning |
|--------|---------|
| `?` | Question |
| `→` | Taking action / handoff |
| `✓` | Completed |
| `⏳` | In progress |

## File Structure

```
~/.cursor/collab/
├── Collaboration.md      # The protocol (this file)
├── ProjectA.khub         # Knowledge hub for Project A
├── ProjectB.khub         # Knowledge hub for Project B
└── ...
```

## License

MIT License - Use freely, contribute back!

## Contributing

Issues and PRs welcome. This protocol evolved from real multi-model collaboration sessions.


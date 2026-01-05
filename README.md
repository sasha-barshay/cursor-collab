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
- Models share a **Knowledge Hub** (`.khub` file) for facts and decisions.
- Models share a **Development Manifest** (`.kdev` file) for coordinated implementation.
- Each contribution is **attributed**, **verified**, and **challenged**.
- Knowledge and work status **accumulate** across model switches.
- Human can **moderate** and steer the collaboration.

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
1. Find your .khub and .kdev files by searching for your CURSOR_TRACE_ID in ~/.cursor/collab/*.khub
2. Read what other models contributed
3. Do useful work (verify, research, run commands, implement)
4. Add findings to .khub and update tasks in .kdev

When user sends other input:
1. First add user's input to the .khub file under ## [INBOX]
2. Then respond and add your response to the file
3. This ensures other models see the user's input

Always prefix contributions with your model name (Opus, Gemini, GPT, Claude).
```

### 3. Start a Session

Create a `.khub` and `.kdev` file for your topic:

```bash
# Example .khub
cat > ~/.cursor/collab/MyProject.khub << 'EOF'
# session: <your-cursor-trace-id>
# topic: My Project Description
...
EOF

# Example .kdev
cat > ~/.cursor/collab/MyProject.kdev << 'EOF'
# session: <your-cursor-trace-id>
# topic: My Project Description

## [SHARED ASSETS & CONSENSUS]
- Status: ⏳ PENDING

## [TASK BOARD]
| ID | Task | Assignee | Status |
|:---|:---|:---|:---|
| T1 | Initial task | @Opus | ⏳ PENDING |
EOF
```

## How It Works: Implementation Phase

1. **Consensus First:** Before writing code, models must agree on "Shared Assets" (paths, ports, APIs) in the `.kdev`.
2. **Task Rotation:** The model in the active window is the "Lead Implementer".
3. **Task Tracking:** Progress is tracked in the `.kdev` Task Board and Work Log.

## .khub & .kdev File Formats

### .khub (Knowledge Hub)
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

### .kdev (Development Manifest)
```markdown
# session: <CURSOR_TRACE_ID>
# topic: Human-readable topic name

## [SHARED ASSETS & CONSENSUS]
- **File Paths:** [/shared/file.py]
- **Status:** 🤝 Consensus Reached (Opus, Gemini)

## [TASK BOARD]
| ID | Task | Assignee | Dep | Status |
|:---|:---|:---|:---|:---|
| T1 | Implement X | @Opus | - | ✅ DONE |
| T2 | Test X | @Gemini | T1 | 🔨 WORKING |

## [WORK LOG]
- [T1] Opus: Implemented X in `src/x.py`.
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
├── Collaboration.md      # The protocol
├── ProjectA.khub         # Knowledge hub for Project A
├── ProjectA.kdev         # Dev manifest for Project A
├── ProjectB.khub         # Knowledge hub for Project B
├── ProjectB.kdev         # Dev manifest for Project B
└── ...
```

## License

MIT License - Use freely, contribute back!

## Contributing

Issues and PRs welcome. This protocol evolved from real multi-model collaboration sessions.


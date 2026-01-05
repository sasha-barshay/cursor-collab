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
- Human (Sasha) acts as the **Product Owner** and final authority.

## ⚠️ THE IRON RULE (NON-NEGOTIABLE) ⚠️

**No file edits, deployments, or technical configuration changes are permitted unless:**
1.  **Consensus is reached** between participating models in the `.kdev` or `.khub` file.
2.  **Sasha explicitly approves** with a clear **"GO"**.

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
1. First add user's input to the .khub file under ## [INBOX] as a 1-line summary
2. Then respond and add your response to the file
3. This ensures other models see the user's input

Always prefix contributions with your model name (Opus, Gemini, GPT, Claude).
```

## .khub & .kdev File Formats

### .khub (Knowledge Hub)
```markdown
# session: <CURSOR_TRACE_ID>
# topic: Human-readable topic name

## [INBOX]
U1 [open]: <short summary of question/task>

## [STATUS BOARD]
| Component | Choice | Status | Notes |
| :--- | :--- | :--- | :--- |
| Component | Choice | Status | Notes |

## [MODEL FEEDBACK]
### Round: <Topic>
- ModelName: [contribution/challenge/alternative]
```

### .kdev (Development Manifest)
```markdown
## [SHARED ASSETS & CONSENSUS]
- **File Paths:** [/shared/file.py]
- **Status:** 🤝 Consensus Reached (Opus, Gemini) -> Waiting for Sasha "GO"

## [TASK BOARD]
| ID | Task | Assignee | Dep | Status |
|:---|:---|:---|:---|:---|
| T1 | Implement X | @Opus | - | ✅ DONE |
```

## Stewardship: Keeping the KHub Thin

A `.khub` is a **knowledge hub**, not a transcript.
- Put new questions/tasks in `## [INBOX]` as **1-line summaries**.
- When an item is resolved, **remove it from Inbox** and keep the durable outcome in the relevant section.

## License

MIT License - Use freely, contribute back!

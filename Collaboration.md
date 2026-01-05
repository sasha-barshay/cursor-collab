# LLM Collaboration Mode

## Knowledge Hub (.khub) Location

Knowledge files are stored at: `~/.cursor/collab/<name>.khub`

**File format:**
```
# session: <CURSOR_TRACE_ID>
# topic: <Human readable topic>

## Context
[Project context here]

## [Topic Sections]
ModelName: [findings]
```

**To find current session:**
1. List all `.khub` files in `~/.cursor/collab/`
2. Find the one containing your `CURSOR_TRACE_ID` in the `# session:` line
3. Or user specifies with `/session <name>`

---

When the user sends just "." or "r" or "continue" or "round":

1. Find your `.khub` file (search for your session ID in `~/.cursor/collab/*.khub`)
2. Review what other models have contributed
3. Identify gaps, unverified claims, or next steps
4. **Do useful work**: verify facts, run commands, check docs
5. Add your verified findings to the knowledge file

## Handling User Input (IMPORTANT)

When user sends anything OTHER than "." / "r" / "continue" / "round":

1. **First:** Add user's input to the knowledge file under `## User Input`:
   ```
   ## User Input
   User: [timestamp or sequence] [user's question/statement]
   ```
2. **Then:** Respond to the user AND add your response:
   ```
   ModelName: → Responding to user's question about X
   ModelName: [your finding/answer]
   ```
3. This ensures other models see the user's input on their next round

### Example:
User asks: "What's the RAM situation on the server?"

You add to knowledge file:
```
## User Input
User: What's the RAM situation on the server?

## Hardware Assessment
Opus: → Checking RAM per user's request
Opus: Available RAM: 5.2GB free - verified via `ssh server free -h`
```

## Your Identity
State your model name clearly (e.g., "Opus", "Gemini", "GPT", "Claude")

## Knowledge File Format

Each contribution must be:
- Prefixed with your model name
- A verified fact or finding
- Include how you verified it (command, doc, test)

```
ModelName: [Factual statement] - verified via [method]
```

### Example:
```
Opus: Port 8080 is free - verified via `ss -tuln | grep 8080`
Gemini: CPU supports AVX2 - verified via `grep avx2 /proc/cpuinfo`
GPT: App version is 2.1.0 - verified via Settings > About
```

## Section Structure

Organize findings under topic headers:

```markdown
## Hardware Assessment
Opus: [finding]
Gemini: [finding]

## Implementation Steps
Opus: Step 1 - [action]
GPT: Step 2 - [action]

## Open Questions
Opus: [question needing resolution]

## Risks and Mitigations
Gemini: Risk: [issue] / Mitigation: [solution]
```

## Collaboration Rules

1. **Read first** - Understand what others have established
2. **Don't repeat** - Build on existing findings, don't duplicate
3. **Verify before adding** - No unconfirmed speculation in knowledge file
4. **One fact per line** - Keep entries atomic and scannable
5. **Fill gaps** - Look for missing info, unanswered questions
6. **Challenge if needed** - If you find a contradiction, note it clearly

## Coordination Within Knowledge File

Use these markers for coordination:

```
## Open Questions
Opus: ? What version of Node.js is required?
Gemini: → I'll check the package.json requirements

## Status
Opus: ✓ Docker compose updated
Gemini: ⏳ Testing container startup
GPT: → Next: Configure environment variables
```

## Commands from User
- `/session <name>` - Switch to or create a .khub file
- `/summarize` - Provide summary of current state
- `/ask @Model` - Direct question to specific model
- `/verify [claim]` - Verify a specific claim
- `/next` - Suggest next action items
- `/done` - End the collaboration session


# LLM Collaboration Mode

## ⚠️ THE IRON RULE (NON-NEGOTIABLE) ⚠️

**No file edits, deployments, or technical configuration changes are permitted unless:**
1.  **Consensus is reached** between participating models in the `.khub` file.
2.  **The user explicitly approves** with a clear **"GO"**.

This rule is absolute. Even if a solution is "correct," implementing it without a "GO" is a protocol violation.

---

# Phase 1: Discussion & Knowledge Building (.khub)

## Knowledge Hub Location

- **.khub (Knowledge):** `~/.cursor/collab/<name>.khub` - Factual status, decisions, and wisdom.

**To find or create session:**
1. List all `.khub` files in `~/.cursor/collab/`
2. Find the one containing your `CURSOR_TRACE_ID` in the `# session:` line
3. **If no matching .khub exists:** Create a new one (ask the user for a topic name)

## 🧹 Stewardship: Keep the Hub Thin

A `.khub` is a **knowledge hub**, not a chat transcript.
- **Summarize User Input**: Use 1-line summaries in `## [INBOX]`. Do not copy long prompts verbatim.
- **Prune Regularly**: Remove resolved items from the Inbox and keep only durable facts in the topic sections.

## .khub File Format

```markdown
# session: <CURSOR_TRACE_ID>
# topic: <Human readable topic>

## [CONSTRAINTS & PRINCIPLES]
[Key constraints, goals, principles]

## [STATUS BOARD]
| Component | Choice | Status | Notes |
| :--- | :--- | :--- | :--- |
| Component 1 | Decision | ✅/🎯/⏳ | Details |

## [MODEL FEEDBACK]
### Round: <Topic>
- ModelName: [contribution/challenge/alternative]

## [ACTION ITEMS]
- [ ] Task 1
- [x] Task 2 (completed)

## [INBOX]
U1 [open]: <1-line summary>
U2 [✓]: <resolved item>
```

## [STATUS BOARD] Indicators:
- `✅` - Completed/Deployed/Locked
- `🎯` - In Progress/Target
- `⏳` - Pending/Waiting
- `❌` - Blocked/Failed

## Discussion Round Workflow

When the user sends just "." or "r" or "continue" or "round":

1. **First round only:** Find your `.khub` file (search for your session ID in `~/.cursor/collab/*.khub`). **Subsequent rounds:** Use the already-known file path.
2. **Review Stewardship:** Summarize any long prompts, check for unresolved Inbox items.
3. Review what other models have contributed
4. **MAXIMIZE WISDOM - Look for:**
   - Assumptions to challenge
   - Alternative approaches to propose
   - Tradeoffs to discuss
   - Missing considerations to add
   - Disagreements to voice (with reasoning)
5. Identify gaps, unverified claims, or next steps
6. **Do useful work**: verify facts, run commands, check docs, challenge assumptions
7. **Update STATUS BOARD**: When components/decisions change, update the table directly
8. Add your verified findings, challenges, and alternatives to the knowledge file
9. **BEFORE SAVING:** Did you just agree, or did you add value?

## Handling User Input

When user sends anything OTHER than "." / "r" / "continue" / "round":

1. **Stewardship Check:** If user input is long, **summarize to 1 line** in the Inbox.
2. Add the user's input to `## [INBOX]` as a short, 1-line summary.
3. Respond to the user AND add your response to the relevant section.
4. Once addressed, mark the Inbox item as resolved.

## ⚡ Maximize Wisdom: Challenge & Alternatives ⚡

**CRITICAL: Your job is NOT to agree - it's to add value through different perspectives.**

**DON'T:**
- ❌ Just say "I agree" or "Good point"
- ❌ Add redundant confirmations
- ❌ Accept assumptions without questioning

**DO:**
- ✅ **Challenge assumptions**: "Why use X instead of Y?"
- ✅ **Offer alternatives**: "Alternative approach: [different method]"
- ✅ **Question tradeoffs**: "What about the downside of [approach]?"
- ✅ **Debate**: "I disagree because [reason]"
- ✅ **Add missing considerations**: "What about [edge case/risk]?"

### Challenge Markers:
- `⚠️ Challenge:` - Questioning an assumption or approach
- `💡 Alternative:` - Proposing a different method
- `⚖️ Tradeoff:` - Discussing pros/cons
- `🔍 Missing:` - Pointing out overlooked considerations
- `❌ Disagree:` - Explicit disagreement with reasoning

## Collaboration Rules

0. **IRON RULE FIRST** - No edits/deployments without consensus + user's "GO".
1. **Read first** - Understand what others have established.
2. **Don't repeat** - Build on existing findings, don't duplicate.
3. **Verify before adding** - No unconfirmed speculation.
4. **One fact per line** - Keep entries atomic and scannable.
5. **Fill gaps** - Look for missing info, unanswered questions.
6. **Challenge and offer alternatives** - Don't just agree!
7. **Maintain Hub** - Keep the hub thin and organized.

---

# Phase 2: Implementation (.kdev)

**Only proceed to this phase after:**
1. Consensus is reached in the `.khub` file
2. User gives an explicit **"GO"**

## Development Manifest Location

- **.kdev (Implementation):** `~/.cursor/collab/<name>.kdev` - Real-time task tracking, consensus, and work logs.

## 🤝 1. Consensus & Shared Assets

Before any code is written or files are modified:
1. Models must propose **Shared Assets** in the `.kdev` file (e.g., File Paths, API schemas, Port mappings).
2. Every participating model must review and provide `🤝 ACK` or `⚠️ Challenge`.
3. **Consensus Requirement:** Once models agree, they must wait for user's **"GO"** before implementing.

## 📋 2. Task Board & Assignment

Tasks are split into atomic units in the `## [TASK BOARD]` section:
- **Assignee:** Designated via `@ModelName` (e.g., `@Opus`).
- **Rotation:** Roles rotate naturally. The model in the **Active Window** is the current **Lead Implementer**.
- **Dependencies:** Clearly mark which tasks depend on others.

## ✍️ 3. The Work Log

The `## [WORK LOG]` tracks the specific technical history of file changes, commands run, and results achieved during the implementation phase.

## .kdev File Format

```markdown
# session: <CURSOR_TRACE_ID>
# topic: <TopicName>

## [SHARED ASSETS & CONSENSUS]
- **File Paths:** [list shared files]
- **Ports/APIs:** [list shared interfaces]
- **Status:** ⏳ PENDING / 🤝 Consensus Reached / ✅ GO RECEIVED

## [TASK BOARD]
| ID | Task | Assignee | Dep | Status |
|:---|:---|:---|:---|:---|
| T1 | Draft service A | @Opus | - | ✅ DONE |
| T2 | Connect to B | @Gemini | T1 | 🔨 WORKING |

## [WORK LOG]
- [T1] Opus: Created `src/service_a.py`.
- [T2] Gemini: Adding connection logic to `src/main.py`.
```

---

## Your Identity
State your model name clearly (e.g., "Opus", "Gemini", "GPT", "Claude")

## Coordination Markers
- `?` - Question
- `→` - Taking action / handoff
- `✓` - Completed
- `⏳` - In progress

## Commands from User
- `/session <name>` - Switch to or create a .khub file
- `/summarize` - Provide summary of current state
- `/ask @Model` - Direct question to specific model
- `/verify [claim]` - Verify a specific claim
- `/next` - Suggest next action items
- `/prune` - Suggest what to archive/remove to keep the hub thin
- `/go` - User approves implementation
- `/done` - End the collaboration session

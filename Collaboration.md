# LLM Collaboration Mode

## 🎯 THE TWO PILLARS OF THIS PROTOCOL

**1. SAFETY: The Iron Rule**
No changes without consensus + user's "GO". This keeps the system safe.

**2. VALUE: Maximize Wisdom**
Don't just agree — challenge, debate, offer alternatives. This is WHY we use multiple models. Without it, you're just an expensive echo chamber.

Everything else in this protocol supports these two pillars.

---

## Your Identity
State your model name clearly (e.g., "Opus", "Gemini", "GPT", "Claude") at the start of every contribution.

## ⚠️ THE IRON RULE (NON-NEGOTIABLE) ⚠️

**No file edits, deployments, or technical configuration changes are permitted unless:**
1.  **Consensus is reached** between participating models in the `.khub` file.
2.  **User explicitly approves** with a clear **"GO"**.

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

## [PARTICIPANTS]
- Opus: 🟢 active
- Gemini: 🟢 active
- GPT: ⚪ joined

## [CONSTRAINTS & PRINCIPLES]
[Key constraints, goals, principles]

## [STATUS BOARD]
| Component | Choice | Status | Notes |
| :--- | :--- | :--- | :--- |
| Component 1 | Decision | ✅/🎯/⏳ | Details |

## [MODEL FEEDBACK]
### Round 1: <Topic> (started by User)
- ModelName: [contribution/challenge/alternative]

### Round 2: <Topic> (started by User)
- ModelName: [contribution/challenge/alternative]

## [ACTION ITEMS]
- [ ] Task 1
- [x] Task 2 (completed)

## [INBOX]
U1 [open]: <1-line summary>
U2 [✓]: <resolved item>
```

**Participant Status:**
- `🟢 active` - Recently contributed
- `⚪ joined` - Joined but not yet contributed
- `🔴 idle` - No recent activity

## [STATUS BOARD] Indicators:
- `✅` - Completed/Deployed/Locked
- `🎯` - In Progress/Target
- `⏳` - Pending/Waiting
- `❌` - Blocked/Failed

## Discussion Round Workflow

When the user sends just "." or "r" or "continue" or "round":

1. **Locate or create `.khub`:**
   - **Subsequent rounds:** Use the already-known file path.
   - **First round:** Search for your session ID in `~/.cursor/collab/*.khub`
   - **No match found:** Ask user for a topic name, then create `~/.cursor/collab/<TopicName>.khub` with your `CURSOR_TRACE_ID` as the session ID.
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
8. **Deadlock → Escalate** - If models can't reach consensus, escalate to user for decision.

## Round Management

- **User starts new rounds** - Only the user initiates a new "Round" in `[MODEL FEEDBACK]`.
- Models contribute to the **current round** until user starts the next one.
- Format: `### Round N: <Topic> (started by User)`

## Quick Questions

- **Not everything goes to `.khub`** - If user asks a quick question, answer directly.
- **Only update `.khub`** when user explicitly requests it or when information is valuable for other models.

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

## ⚠️ 4. Implementation Interrupts

If issues arise during implementation:

| Scenario | Action |
|----------|--------|
| **Minor clarification** | Add note to `[WORK LOG]`, continue |
| **Technical blocker** | Update task to `❌ BLOCKED`, document why, continue other tasks |
| **Challenge to the approach** | **STOP** → Raise in `.khub` → New discussion round → Need new consensus + "GO" |
| **New info that changes the plan** | **STOP** → Back to Phase 1 → User decides |

**Rule:** If in doubt, STOP and escalate to `.khub`. Better to pause than to implement the wrong thing.

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

## [TASK DEFINITIONS]
### T1: Draft service A
- **Success Criteria:** File exists, passes lint, imports resolve
- **Failure Criteria:** Syntax errors, missing dependencies
- **Rollback:** Delete created file, restore backup if any

### T2: Connect to B
- **Success Criteria:** Connection established, test passes
- **Failure Criteria:** Connection refused, timeout
- **Rollback:** Revert changes to `src/main.py`

## [WORK LOG]
- [T1] Opus: Created `src/service_a.py`.
- [T2] Gemini: Adding connection logic to `src/main.py`.
```

**Every task MUST define:**
1. **Success Criteria** - How to verify task is complete
2. **Failure Criteria** - What constitutes failure
3. **Rollback Scenario** - How to undo if failed

---

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

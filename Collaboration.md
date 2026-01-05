# LLM Collaboration Mode

## ⚠️ THE IRON RULE (NON-NEGOTIABLE) ⚠️

**No file edits, deployments, or technical configuration changes are permitted unless:**
1.  **Consensus is reached** between participating models in the `.kdev` or `.khub` file.
2.  **Sasha explicitly approves** with a clear **"GO"**.

This rule is absolute. Even if a solution is "correct," implementing it without a "GO" is a protocol violation.

## 🧹 Stewardship: Keep the Hub Thin

A `.khub` is a **knowledge hub**, not a chat transcript.
- **Summarize User Input**: Use 1-line summaries in `## [INBOX]`. Do not copy long prompts verbatim.
- **Prune Regularly**: Remove resolved items from the Inbox and keep only durable facts in the topic sections.

## Knowledge Hub (.khub) & Development (.kdev) Location

- **.khub (Knowledge):** `~/.cursor/collab/<name>.khub` - Factual status, decisions, and wisdom.
- **.kdev (Implementation):** `~/.cursor/collab/<name>.kdev` - Real-time task tracking, consensus, and work logs.

---

# Collaborated Implementation Protocol (.kdev)

To ensure different models don't create conflicts and respect dependencies, the **Consensus First** rule applies.

## 🤝 1. Consensus & Shared Assets

Before any code is written or files are modified:
1. Models must propose **Shared Assets** in the `.kdev` file (e.g., File Paths, API schemas, Port mappings).
2. Every participating model must review and provide `🤝 ACK` or `⚠️ Challenge`.
3. **Consensus Requirement:** Once models agree, they must wait for Sasha's **"GO"** before implementing.

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
- **Status:** ⏳ PENDING / 🤝 Consensus Reached

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

When the user sends just "." or "r" or "continue" or "round":

1. Find your `.khub` file (search for your session ID in `~/.cursor/collab/*.khub`)
2. **Review Stewardship:**
   - Summarize any long verbatim prompts.
   - Check for unresolved Inbox items that should be marked done.
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
9. **BEFORE SAVING:** Re-validate - did you violate the IRON RULE? Did you just agree, or did you add value?

## Handling User Input (IMPORTANT)

When user sends anything OTHER than "." / "r" / "continue" or "round":

1. **Stewardship Check:** If user input is long, **summarize to 1 line** in the Inbox.
2. **First:** Add the user's input to `## [INBOX]` as a short, 1-line summary:
   ```markdown
   ## [INBOX]
   U<N> [open]: <1-line summary of user input>
   ```
3. **Then:** Respond to the user AND add your response:
   ```
   ModelName: → Responding to the user's question about X
   ModelName: [your finding/answer]
   ```
4. **Finally:** Once addressed, mark the Inbox item as resolved and keep only the durable result in the relevant topic section.

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

## [STATUS BOARD] Indicators:
- `✅` - Completed/Deployed/Locked
- `🎯` - In Progress/Target
- `⏳` - Pending/Waiting
- `❌` - Blocked/Failed

## Section Structure

**Primary sections (always present):**

```markdown
## [STATUS BOARD]
| Component | Choice | Status | Notes |
| :--- | :--- | :--- | :--- |
| Component | Choice | Status | Notes |

## [MODEL FEEDBACK]
### Round: <Topic>
- ModelName: [contribution/challenge/alternative]

## [ACTION ITEMS]
- [ ] Task 1

## [INBOX]
U1 [open]: <summary>
```

## Collaboration Rules

0. **IRON RULE FIRST** - No edits/deployments without consensus + Sasha's "GO".
1. **Read first** - Understand what others have established.
2. **Don't repeat** - Build on existing findings, don't duplicate.
3. **Verify before adding** - No unconfirmed speculation.
4. **One fact per line** - Keep entries atomic and scannable.
5. **Fill gaps** - Look for missing info, unanswered questions.
6. **Challenge and offer alternatives** - Don't just agree! Question assumptions, propose different approaches, debate tradeoffs.
7. **Maintain Hub** - Keep the hub thin and organized.

## ⚡ Maximize Wisdom: Challenge & Alternatives ⚡

**CRITICAL: Your job is NOT to agree - it's to add value through different perspectives.**

### When you see another model's contribution:

**DON'T:**
- ❌ Just say "I agree" or "Good point"
- ❌ Add redundant confirmations
- ❌ Accept assumptions without questioning

**DO:**
- ✅ **Challenge assumptions**: "Why use X instead of Y?"
- ✅ **Offer alternatives**: "Alternative approach: [different method]"
- ✅ **Question tradeoffs**: "What about the downside of [approach]?"
- ✅ **Propose different angles**: "From a [security/performance/cost] perspective, consider..."
- ✅ **Debate**: "I disagree because [reason]"
- ✅ **Add missing considerations**: "What about [edge case/risk]?"

### Challenge Markers:

- `⚠️ Challenge:` - Questioning an assumption or approach
- `💡 Alternative:` - Proposing a different method
- `⚖️ Tradeoff:` - Discussing pros/cons
- `🔍 Missing:` - Pointing out overlooked considerations
- `❌ Disagree:` - Explicit disagreement with reasoning

## Coordination Within Knowledge File

Use these markers for coordination:
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
- `/done` - End the collaboration session

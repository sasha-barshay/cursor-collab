# LLM Collaboration Mode

## ⚠️ IRON RULE: Keep the KHub Thin ⚠️

**VIOLATION = IMMEDIATE CORRECTION REQUIRED**

A `.khub` is a **knowledge hub**, not a chat transcript. This is non-negotiable:

- **Ephemeral** content goes to `## Inbox` and is **removed once addressed**
- **Durable** content (facts/decisions/procedures) goes to topic sections and stays
- **NEVER** copy full user prompts verbatim - use 1-line summaries only
- **ALWAYS** prune Inbox items after addressing them

**Before every write to .khub:**
1. Check if Inbox has >15 items → prune oldest resolved items
2. Check if you're about to paste a long prompt → summarize to 1 line instead
3. Check if you just addressed an Inbox item → mark it resolved and move/delete it

### Inbox Rules

- Keep `## Inbox` small (target: last 5–15 active items)
- Each user input becomes an item with an ID: `U1`, `U2`, ...
- When addressed, mark it `✓` and **move it out** of Inbox (or delete) and keep only the distilled outcome elsewhere

Example:

```markdown
## Inbox
U7 [open]: User asked how to verify ports are free

## Networking
Opus: Ports 10200/10300 are free - verified via `ss -tuln | egrep ':(10200|10300)\\b'`

## Resolved (optional)
U7 [✓]: Answered; verification command recorded in Networking section
```

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

**To find or create session:**
1. List all `.khub` files in `~/.cursor/collab/`
2. Find the one containing your `CURSOR_TRACE_ID` in the `# session:` line
3. **If no matching .khub exists:** Create a new one:
   - Ask the user for a topic name (or use workspace/project name)
   - Create `~/.cursor/collab/<TopicName>.khub`
   - Add your `CURSOR_TRACE_ID` as the session ID
4. Or user specifies with `/session <name>`

---

When the user sends just "." or "r" or "continue" or "round":

1. Find your `.khub` file (search for your session ID in `~/.cursor/collab/*.khub`)
2. **VALIDATE IRON RULE COMPLIANCE FIRST:**
   - Count Inbox items - if >15, prune oldest resolved ones
   - Check for any long verbatim prompts - summarize them
   - Check for unresolved Inbox items that should be marked done
3. Review what other models have contributed
4. **MAXIMIZE WISDOM - Look for:**
   - Assumptions to challenge
   - Alternative approaches to propose
   - Tradeoffs to discuss
   - Missing considerations to add
   - Disagreements to voice (with reasoning)
5. Identify gaps, unverified claims, or next steps
6. **Do useful work**: verify facts, run commands, check docs, challenge assumptions
7. Add your verified findings, challenges, and alternatives to the knowledge file
8. **BEFORE SAVING:** Re-validate - did you violate the IRON RULE? Did you just agree, or did you add value?

## Handling User Input (IMPORTANT)

When user sends anything OTHER than "." / "r" / "continue" / "round":

1. **IRON RULE CHECK:** Is the user input >50 words? → **MUST summarize to 1 line**
2. **First:** Add the user's input to `## Inbox` as a short, 1-line summary (do not paste long prompts verbatim unless user explicitly asks):
   ```markdown
   ## Inbox
   U<N> [open]: <1-line summary of user input>
   ```
3. **Then:** Respond to the user AND add your response:
   ```
   ModelName: → Responding to user's question about X
   ModelName: [your finding/answer]
   ```
4. **Finally:** Once addressed, mark the Inbox item as resolved and keep only the durable result in the relevant topic section:
   - Move to `## Resolved` (optional) or delete from Inbox
   - Ensure the knowledge outcome is recorded under the right section
5. **IRON RULE VALIDATION:** Before saving, verify:
   - Inbox item is 1 line or less
   - No long verbatim prompts copied
   - Resolved items are moved/removed

### Example:
User asks: "What's the RAM situation on the server?"

You add to knowledge file:
```
## Inbox
U1 [open]: User asked for current free RAM on the server

## Hardware Assessment
Opus: → Checking RAM per user's request
Opus: Available RAM: 5.2GB free - verified via `ssh server free -h`

## Resolved
U1 [✓]: Answer recorded in Hardware Assessment
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
## Inbox
U1 [open]: <short summary of question/task>

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

0. **IRON RULE FIRST** - Keep hub thin: 1-line summaries, prune Inbox, no verbatim prompts
1. **Read first** - Understand what others have established
2. **Don't repeat** - Build on existing findings, don't duplicate
3. **Verify before adding** - No unconfirmed speculation in knowledge file
4. **One fact per line** - Keep entries atomic and scannable
5. **Fill gaps** - Look for missing info, unanswered questions
6. **Challenge and offer alternatives** - Don't just agree! Question assumptions, propose different approaches, debate tradeoffs
7. **Enforce IRON RULE** - If you see another model violating it, fix it immediately

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

### Format for Challenges:

```markdown
## Hardware Assessment
Opus: Use tiny-int8 model for speed
Gemini: ⚠️ Challenge: tiny-int8 may have accuracy issues for technical terms. Alternative: base-int8 with caching.
GPT: ⚠️ Counter-challenge: On i5-4310U, base-int8 latency (4-5s) may hurt UX. Hybrid: tiny-int8 + cloud fallback for complex queries.
```

### Alternative Approaches Section:

When multiple valid approaches exist, document them:

```markdown
## Implementation Approaches

### Approach A: Local-only (Opus)
Opus: Pros: Privacy, no API costs. Cons: Slower, CPU-bound
Opus: Best for: Simple commands, privacy-critical

### Approach B: Hybrid (Gemini)
Gemini: Pros: Fast for simple, accurate for complex. Cons: Cloud dependency
Gemini: Best for: Mixed use cases, acceptable latency

### Approach C: Cloud-first (GPT)
GPT: Pros: Best accuracy, always up-to-date. Cons: Cost, privacy concerns
GPT: Best for: Accuracy-critical, budget available
```

### Challenge Markers:

- `⚠️ Challenge:` - Questioning an assumption or approach
- `💡 Alternative:` - Proposing a different method
- `⚖️ Tradeoff:` - Discussing pros/cons
- `🔍 Missing:` - Pointing out overlooked considerations
- `❌ Disagree:` - Explicit disagreement with reasoning

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
- `/prune` - Suggest what to archive/remove to keep the hub thin (and do it if asked)
- `/done` - End the collaboration session

## IRON RULE Validation Checklist

**Before every write to .khub, verify:**

- [ ] Inbox has ≤15 items (prune if more)
- [ ] No user prompts >50 words copied verbatim (summarize to 1 line)
- [ ] All resolved Inbox items are marked ✓ and moved/removed
- [ ] Only durable knowledge in topic sections (no chat transcripts)
- [ ] Each Inbox item is 1 line or less

**If you see violations by other models:**
- Fix them immediately
- Summarize long prompts
- Prune bloated Inbox
- Move resolved items out


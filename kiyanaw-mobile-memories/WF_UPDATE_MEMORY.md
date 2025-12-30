# WF_UPDATE_MEMORY - Update Memory

> **📝 On step WF_UPDATE_MEMORY**

⬆️ OUTPUT THE ABOVE LINE IMMEDIATELY. Do not read further until you have reported your step to the user.

---

## When To Use

- Adding new requirement to feature memory
- Updating existing requirement after clarification
- Creating new feature memory that doesn't exist
- Updating WORKING_MEMORY with task progress
- **Capturing architectural decisions after WF_ARCH_REVIEW or WF_PLAN_ARCHITECTURE**

## Execute These Steps

IMPORTANT: ONLY WRITE THE MEMORY FOR THIS TASK

1. **Identify the target memory:**
   - Feature requirement → `FEATURE_[X]`
   - Architecture change → `ARCH_[LAYER]`
   - Working state → `WORKING_MEMORY`

2. **For FEATURE_[X] memories, include:**
   - User-facing behavior description
   - Any constraints or edge cases
   - Related files/symbols if known

3. **For WORKING_MEMORY, use this format:**

```markdown
## Current Task
**[STATUS]**: [Task Name]

### Context (enough to resume fresh)
[1-2 sentences: what we're doing and why]

### Mode: Implementation
- [ ] Step 1
- [x] Step 2 (completed)

**Decisions:** [Key decisions and rationale]

**Architecture Context:** (for multi-layer work)
- Layers: [Service, Use-Case, Hook, etc.]
- ARCH_* read: [ARCH_SERVICES, ARCH_USECASES, etc.]
- Key constraints: [e.g., "Services return ADTs", "Use-cases orchestrate"]

**Files/Focus:** `path/to/file.ts` - [what's happening here]

### Mode: Bug Fix
**Symptoms:** [What's broken, error messages]
**What Works:** [Things still working - helps narrow down]
**What Didn't Work:** [Attempted fix] - [why it failed]
**Previous State:** [If modified for testing]
**Files/Focus:** `path/to/file.ts:123` - [problem area]

## Previous Task
**[OUTCOME]**: [Task name] - [2-3 line summary only]
```

4. **For architecture snapshots (after reading ARCH_* memories), include:**
   - Which layers are involved
   - Which ARCH_* and TP_* were consulted
   - Key constraints that apply to this task
   - This enables proper resume after session breaks

5. **Use Serena to update:**
   ```
   mcp__serena__write_memory("FEATURE_[X]", "content")
   mcp__serena__edit_memory("FEATURE_[X]", "old", "new", "literal")
   ```

6. **Confirm to user:**
   "Updated [MEMORY_NAME] with: [brief description]"

## ⛔ MANDATORY NEXT STEP

**YOU ARE NOT FINISHED.** Before responding to user:

| Condition | MUST Read Next |
|-----------|----------------|
| Feature memory updated | `WF_LOAD_FEATURE` |
| WORKING_MEMORY updated | Return to previous workflow step |

1. Read the appropriate WF_* memory NOW
2. Report the new step to user

**SKIPPING THIS TRANSITION = WORKFLOW VIOLATION**

[CRITICAL: Are you on a WF_* workflow step? Did you report on it?]

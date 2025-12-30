# WF_CHECKPOINT - Update Progress

> **✅ On step WF_CHECKPOINT**

⬆️ OUTPUT THE ABOVE LINE IMMEDIATELY. Do not read further until you have reported your step to the user.

---

## Execute These Steps

1. **Update WORKING_MEMORY using the format below**
2. **Keep only ONE Previous Task** (brief, 2-3 lines max)

## WORKING_MEMORY Format

```markdown
# Working Memory - kiyânaw

## Current Task
**[STATUS]**: [Task Name]

### Context (enough to resume fresh)
[1-2 sentences: what we're doing and why]

### Mode: Implementation
- [ ] Step 1
- [x] Step 2 (completed)
- [ ] Step 3

**Decisions:**
- [Key decision and rationale]

**Architecture Context:** (for multi-layer work)
- Layers: [Service, Use-Case, Hook, etc.]
- ARCH_* read: [ARCH_SERVICES, ARCH_USECASES, etc.]
- Key constraints: [e.g., "Services return ADTs", "Use-cases orchestrate"]

**Files/Focus:**
- `path/to/file.ts` - [what's happening here]
- Symbol: `ClassName.methodName` - [focus point]

### Mode: Bug Fix
**Symptoms:** [What's broken, error messages]

**What Works:**
- [Thing that still works - helps narrow down]

**What Didn't Work:**
- [Attempted fix] - [why it failed]

**Previous State:** (if modified for testing)
- [What was changed temporarily]

**Files/Focus:**
- `path/to/file.ts:123` - [problem area]

## Previous Task
**[OUTCOME]**: [Task name] - [2-3 line summary of what was done]
```

## Triggers for this state
- Created/deleted a file
- Modified multiple symbols
- Completed a phase
- ~5 minutes elapsed

## ⛔ MANDATORY NEXT STEP

**YOU ARE NOT FINISHED.** Before responding to user:

| Condition | MUST Read Next |
|-----------|----------------|
| More work remains | `WF_EXECUTE` |
| All work complete | `WF_VERIFY` |

1. Determine which condition applies
2. Read that WF_* memory NOW
3. Report the new step to user

**SKIPPING THIS TRANSITION = WORKFLOW VIOLATION**

[CRITICAL: Are you on a WF_* workflow step? Did you report on it?]

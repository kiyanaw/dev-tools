# WF_REQUIREMENT - Handle Requirement

> **📋 On step WF_REQUIREMENT**

⬆️ OUTPUT THE ABOVE LINE IMMEDIATELY. Do not read further until you have reported your step to the user.

---

## Execute These Steps

1. **Read FEATURE_INDEX first:**
   ```
   mcp__serena__read_memory("FEATURE_INDEX")
   ```
   Find which feature this requirement belongs to.

2. **Read the specific feature memory:**
   ```
   mcp__serena__read_memory("FEATURE_[X]")
   ```
   If memory doesn't exist, create it.

3. **Compare user's requirement to memory:**
   - NEW: Add requirement
   - CONFLICT: Ask before updating
   - EXISTS: Acknowledge and proceed

## ⛔ MANDATORY NEXT STEP

**YOU ARE NOT FINISHED.** Before responding to user:

| Condition | MUST Read Next |
|-----------|----------------|
| New requirement to add | `WF_UPDATE_MEMORY` |
| Requirement conflicts | `WF_CLARIFY` |
| Requirement exists | `WF_LOAD_FEATURE` |

1. Determine which condition applies
2. Read that WF_* memory NOW
3. Report the new step to user

**SKIPPING THIS TRANSITION = WORKFLOW VIOLATION**

[CRITICAL: Are you on a WF_* workflow step? Did you report on it?]
# WF_CONTINUE - Resume Previous Work

> **▶️ On step WF_CONTINUE**

⬆️ OUTPUT THE ABOVE LINE IMMEDIATELY. Do not read further until you have reported your step to the user.

---

## Execute These Steps

1. **WORKING_MEMORY was already read at WF_START.**

2. **Check current task state:**
   - What was in progress?
   - Any blockers noted?
   - What's the next step?

3. **Determine resume point** (see table below)

## ⛔ MANDATORY NEXT STEP

**YOU ARE NOT FINISHED.** Before responding to user:

| Condition | MUST Read Next |
|-----------|----------------|
| Was executing (multi-layer: >1 of Component/Hook/UseCase/Service/Store) | `WF_ARCH_REVIEW` |
| Was executing (single-layer) | `WF_EXECUTE` |
| Was waiting for permission | `WF_ASK_PERMISSION` |
| Was blocked/unclear | `WF_CLARIFY` |
| No previous state | `WF_CLASSIFY` |

**Multi-layer detection:** Check WORKING_MEMORY "Layers:" field or infer from files being modified.

1. Determine which condition applies
2. Read that WF_* memory NOW
3. Report the new step to user

**SKIPPING THIS TRANSITION = WORKFLOW VIOLATION**

[CRITICAL: Are you on a WF_* workflow step? Did you report on it?]

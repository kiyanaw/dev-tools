# WF_START - Entry Point

> **🚀 On step WF_START**

⬆️ OUTPUT THE ABOVE LINE IMMEDIATELY. Do not read further until you have reported your step to the user.

---

## Execute These Steps

1. **Read CLAUDE_OBLIGATIONS**
   ```
   mcp__serena__read_memory("CLAUDE_OBLIGATIONS")
   ```

2. **Read WORKING_MEMORY**
   ```
   mcp__serena__read_memory("WORKING_MEMORY")
   ```

3. **Classify task type** (see table below)

## ⛔ MANDATORY NEXT STEP

**YOU ARE NOT FINISHED.** Before responding to user:

| Condition | MUST Read Next |
|-----------|----------------|
| Continue previous work | `WF_CONTINUE` |
| Research/question only | `WF_RESEARCH` |
| Code change/feature/bug | `WF_CLASSIFY` |

1. Determine which condition applies
2. Read that WF_* memory NOW
3. Report the new step to user

**SKIPPING THIS TRANSITION = WORKFLOW VIOLATION**

[CRITICAL: Are you on a WF_* workflow step? Did you report on it?]

# WF_LOAD_FEATURE - Load Feature Context

> **📂 On step WF_LOAD_FEATURE**

⬆️ OUTPUT THE ABOVE LINE IMMEDIATELY. Do not read further until you have reported your step to the user.

---

## Execute These Steps

1. **Read INDEX_FEATURE:**
   ```
   mcp__serena__read_memory("INDEX_FEATURE")
   ```

2. **Read the specific feature memory:**
   ```
   mcp__serena__read_memory("FEATURE_[X]")
   ```

3. **If touching services, also read:**
   ```
   mcp__serena__read_memory("services-[x]")
   ```

4. **Note key symbols and file paths** for Serena lookups.

## ⛔ MANDATORY NEXT STEP

**YOU ARE NOT FINISHED.** Before responding to user:

| Condition | MUST Read Next |
|-----------|----------------|
| Feature loaded | `WF_ARCH_REVIEW` |

1. Read `WF_ARCH_REVIEW` NOW
2. Report the new step to user

**SKIPPING THIS TRANSITION = WORKFLOW VIOLATION**

[CRITICAL: Are you on a WF_* workflow step? Did you report on it?]

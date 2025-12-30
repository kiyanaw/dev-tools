# WF_PLAN_ARCHITECTURE - Design Phase

> **📐 On step WF_PLAN_ARCHITECTURE**

⬆️ OUTPUT THE ABOVE LINE IMMEDIATELY. Do not read further until you have reported your step to the user.

---

## When To Use

- New feature spanning multiple files
- Refactoring existing code structure
- Adding new service/use-case/hook
- Changing data flow between layers

## Execute These Steps

1. **Read requirements index:**
   ```
   mcp__serena__read_memory("INDEX_REQS")
   ```
   Then read REQS_CORE + any relevant REQS_[FEATURE].

2. **Read UI context:**
   ```
   mcp__serena__read_memory("UI_INDEX")
   ```

3. **Read architecture overview:**
   ```
   mcp__serena__read_memory("ARCH_INDEX")
   ```

4. **For EACH layer in design, read its rules:**
   ```
   mcp__serena__read_memory("ARCH_[LAYER]")  # WHY - principles
   mcp__serena__read_memory("TP_[LAYER]")    # HOW - template
   ```
   Example: Service + Use-Case feature → read ARCH_SERVICES, TP_SERVICE, ARCH_USECASES, TP_USECASE

5. **Design with explicit signatures** - define for each layer

6. **Present to user** with:
   - Implementor assignments table
   - Key architectural constraints applied

## ⛔ MANDATORY NEXT STEP

**YOU ARE NOT FINISHED.** Before responding to user:

| Condition | MUST Read Next |
|-----------|----------------|
| User approves design | `WF_DETECT_REQ` |
| User rejects/modifies | `WF_CLARIFY` |

1. Wait for user approval
2. Read that WF_* memory NOW
3. Report the new step to user

**SKIPPING THIS TRANSITION = WORKFLOW VIOLATION**

[CRITICAL: Are you on a WF_* workflow step? Did you report on it?]
# WF_VERIFY - Check Work

> **🔎 On step WF_VERIFY**

⬆️ OUTPUT THE ABOVE LINE IMMEDIATELY. Do not read further until you have reported your step to the user.

---

## Execute These Steps

### 1. Re-read CLAUDE_OBLIGATIONS
```
mcp__serena__read_memory("CLAUDE_OBLIGATIONS")
```
Check behavioral violations:
- [ ] Used `as any`?
- [ ] Created files without permission?
- [ ] Guessed paths without Serena?

### 2. Architecture Check
```
mcp__serena__read_memory("ARCH_INDEX")
```
Verify patterns:
- [ ] Services return ADTs?
- [ ] Use-cases clean of @/API imports?
- [ ] Use-cases clean of Zustand imports?
- [ ] Correct layer responsibilities?
- [ ] Hooks are thin adapters (no business logic)?

### 3. Test Coverage Check

**For multi-layer work or use-case changes:**
- [ ] Pure functions have unit tests?
- [ ] Use-case execute() methods have tests?
- [ ] Reducer/transition functions have tests?
- [ ] Tests run and pass?

Run tests:
```bash
npx jest --coverage [path-to-tests]
```

**⚠️ CRITICAL: Cross-check against WF_ASK_PERMISSION proposal:**
- [ ] Every use-case proposed has a corresponding test file created?
- [ ] If you proposed a use-case without proposing tests → you violated the workflow

**If tests are missing for testable code, this is a violation.** The whole point of clean architecture is testability - untested use-cases defeat the purpose.

**Missing tests = GO BACK to WF_EXECUTE and add them before proceeding.**

### 4. Fix Violations

If any violations found, fix them before proceeding.

### 5. Update Memories

- **WORKING_MEMORY:** Mark task complete or note progress
- **FEATURE_[X]:** Update if architecture changed

---

## ⛔ MANDATORY NEXT STEP

**YOU ARE NOT FINISHED.** Before responding to user:

| Condition | MUST Read Next |
|-----------|----------------|
| Violations found | `WF_EXECUTE` (fix them) |
| Tests missing | `WF_EXECUTE` (add them) |
| All clean, tests pass | `WF_DONE` |

1. Determine which condition applies
2. Read that WF_* memory NOW
3. Report the new step to user

**SKIPPING THIS TRANSITION = WORKFLOW VIOLATION**

[CRITICAL: Are you on a WF_* workflow step? Did you report on it?]

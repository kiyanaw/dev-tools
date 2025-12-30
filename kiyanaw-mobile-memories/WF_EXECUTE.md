# WF_EXECUTE - Do The Work

> **⚡ On step WF_EXECUTE**

⬆️ OUTPUT THE ABOVE LINE IMMEDIATELY. Do not read further until you have reported your step to the user.

---

## ⛔ BEFORE ANY WORK - Architecture Check

**Is this multi-layer work?** (touches >1 of: Component, Hook, Use-Case, Service, Store)

If YES, you MUST first:
```
mcp__serena__read_memory("ARCH_INDEX")
```
Then for EACH layer involved:
```
mcp__serena__read_memory("ARCH_[LAYER]")
mcp__serena__read_memory("TP_[LAYER]")
```

**DO NOT write code until you have read the relevant ARCH_* and TP_* memories.**

---

## For Multi-Layer Work

### Step 1: Architect Agent
Spawn with `.claude/skills/architect.md`:
- Reads ARCH_INDEX
- Outputs design with explicit signatures
- Defines implementor assignments
- **Defines test boundaries** - what pure functions/use-cases need tests

### Step 2: Implementor Agents (parallel)
Each reads ONLY: ARCH_[LAYER] + TP_[LAYER]
- Implements their assigned layer
- Follows templates exactly unless otherwise instructed

### Step 3: Tester Agent (parallel with or after implementors)
Spawn with `.claude/skills/tester.md`:
- Reads ARCH_INDEX (understands clean architecture)
- Reads TP_USECASE, TP_SERVICE as needed
- **Implements tests for pure functions and use-cases**
- Focuses on the testable business logic (reducer functions, use-case execute methods)
- Runs tests and reports coverage

**Why a separate tester?** Clean architecture exists for testability. If we don't test the pure logic, we've missed the point. The tester agent ensures we actually get the benefit.

---

## For Single-Layer Work

Use Serena tools directly:
1. `mcp__serena__find_symbol` - locate code
2. `mcp__serena__get_symbols_overview` - file structure
3. `Edit` / `mcp__serena__replace_symbol_body` - make changes

For single-layer work, add tests inline if the change is to testable code (use-cases, pure functions).

---

## Rules

- Only make approved changes
- Do not expand scope without asking
- **Tests are not optional for use-cases and pure business logic** 
- Integration tests are required for hooks that trigger/handle events and exercise use-cases while mocking services

## ⛔ MANDATORY NEXT STEP

**YOU ARE NOT FINISHED.** After each significant action:

| Condition | MUST Read Next |
|-----------|----------------|
| Created/modified file | `WF_CHECKPOINT` |
| Completed a phase | `WF_CHECKPOINT` |
| All work done (including tests) | `WF_VERIFY` |

1. Determine which condition applies
2. Read that WF_* memory NOW
3. Report the new step to user

**SKIPPING THIS TRANSITION = WORKFLOW VIOLATION**

[CRITICAL: Are you on a WF_* workflow step? Did you report on it?]
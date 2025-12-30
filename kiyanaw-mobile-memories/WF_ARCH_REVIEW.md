# WF_ARCH_REVIEW - Architecture Compliance Check

> **🏗️ On step WF_ARCH_REVIEW**

⬆️ OUTPUT THE ABOVE LINE IMMEDIATELY. Do not read further until you have reported your step to the user.

---

## Execute These Steps

1. **Read ARCH_INDEX:**
   ```
   mcp__serena__read_memory("ARCH_INDEX")
   ```

2. **Identify layers touched** by proposed change:
   - Component, Hook, Use-Case, Service, Store, ADT?

3. **For each layer, read its template:**
   ```
   mcp__serena__read_memory("TP_[LAYER]")
   ```

4. **Answer these questions:**
   - [ ] Which layer OWNS this logic?
   - [ ] Am I putting logic in the correct layer?
   - [ ] Am I bypassing any layers (e.g., component → service)?

## ⛔ STOP CONDITIONS

**If any of these are true, REDESIGN before proceeding:**

### Backend Layers
- Business logic in component
- Service called directly from component
- Hook contains business logic (not just callback wrapper)
- Use-case imports React

### UI Layer (check EVERY route file in `app/`)
- Route file contains `useState`
- Route file contains `useEffect` or `useCallback`
- Route file has handler definitions (`const handleX = () => {}`)
- Route file has helper functions (`function helperFn()`)
- Route file has `StyleSheet.create()`
- Route file imports services directly
- Route file defines types/interfaces
- Route file passes props to components (components should use hooks)
- Route file is more than ~30 lines

**See ARCH_ROUTES for correct pattern.**

## ⛔ MANDATORY NEXT STEP

| Condition | MUST Read Next |
|-----------|----------------|
| Approach is compliant | `WF_ASK_PERMISSION` |
| Needs redesign | Loop back, fix approach |

**SKIPPING THIS TRANSITION = WORKFLOW VIOLATION**

[CRITICAL: Are you on a WF_* workflow step? Did you report on it?]

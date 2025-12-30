# WF_ASK_PERMISSION - Get Approval

> **❓ On step WF_ASK_PERMISSION**

⬆️ OUTPUT THE ABOVE LINE IMMEDIATELY. Do not read further until you have reported your step to the user.

---

## Template Check (For New Files)

Before proposing new files:
1. Read `TP_INDEX_LOOKUP` memory
2. Match file pattern → template
3. Template exists?
   - Yes → "Will follow TP_[TYPE]"
   - No → "No template for this type. Create one?"

## MANDATORY - Ask User Before Any Code Changes

**Format (must include architecture justification):**
```
I plan to:
- Modify: [file] → [layer] (per TP_[LAYER]: [rule being followed])
- Create: [file] → [layer] (following TP_[LAYER])

Data flow: Component → Hook → Use-Case → Service → Store
          (or simpler subset if applicable)

Proceed? (yes/no)
```

**Example:**
```
I plan to:
- Create: useIsInPlaylist hook → Hook (per TP_HOOK: state selector, no logic)
- Modify: phrase/[id].tsx → Component (per TP_COMPONENT: UI only, uses hook)

Data flow: Component → Hook → Store (currentUser.playlists)

Proceed?
```

## ⚠️ TEST FILE ENFORCEMENT

**For every use-case or pure function proposed, you MUST also propose a corresponding test file.**

Before finalizing your proposal, check:
- [ ] Each new use-case has a `__tests__/[name].test.ts` file proposed
- [ ] Each pure function/reducer has test coverage proposed

**If proposing a use-case without its test file = WORKFLOW VIOLATION**

The whole point of clean architecture is testability. Untested use-cases defeat the purpose.

## ⛔ MANDATORY NEXT STEP

**YOU ARE NOT FINISHED.** Before responding to user:

| Condition | MUST Read Next |
|-----------|----------------|
| User says yes | `WF_EXECUTE` |
| User says no | `WF_CLARIFY` |

1. Wait for user response
2. Read that WF_* memory NOW
3. Report the new step to user

**SKIPPING THIS TRANSITION = WORKFLOW VIOLATION**

[CRITICAL: Are you on a WF_* workflow step? Did you report on it?]

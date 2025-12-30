# Template: Workflow State (WF_*)

## Structure

```markdown
# WF_[NAME] - [Brief Description]

> **[ICON] On step WF_[NAME]**

⬆️ OUTPUT THE ABOVE LINE IMMEDIATELY. Do not read further until you have reported your step to the user.

---

## Execute These Steps
1. [Action]
2. [Action]

## ⛔ MANDATORY NEXT STEP

**YOU ARE NOT FINISHED.** Before responding to user:

| Condition | MUST Read Next |
|-----------|----------------|
| [condition] | `WF_[NEXT]` |
| [condition] | `WF_[OTHER]` |

1. Determine which condition applies
2. Read that WF_* memory NOW
3. Report the new step to user

**SKIPPING THIS TRANSITION = WORKFLOW VIOLATION**

[CRITICAL: Are you on a WF_* workflow step? Did you report on it?]
```

## Icon Reference
| Step Type | Icon |
|-----------|------|
| Entry/Start | 🚀 |
| Continue | ▶️ |
| Classify | 🔍 |
| Architecture | 📐 |
| Research | 🔬 |
| Requirement | 📋 |
| Permission | ❓ |
| Execute | ⚡ |
| Checkpoint | ✅ |
| Verify | 🔎 |
| Clarify | 💬 |
| Update | 📝 |
| Done | 🏁 |

## Rules

- **Report line FIRST** - Before the separator, forces immediate output
- **Max 25 lines** - Split if longer
- **One responsibility** - Each state does one thing
- **Explicit transitions** - Always declare next states with MANDATORY block
- **Blocking language required** - Every step except WF_DONE

## Checklist

- [ ] Report line is FIRST (before ---)?
- [ ] Has "OUTPUT IMMEDIATELY" instruction?
- [ ] Under 25 lines after separator?
- [ ] Has ⛔ MANDATORY NEXT STEP section?
- [ ] Condition table populated?
- [ ] "WORKFLOW VIOLATION" warning present?

[CRITICAL: Are you on a WF_* workflow step? Did you report on it?]

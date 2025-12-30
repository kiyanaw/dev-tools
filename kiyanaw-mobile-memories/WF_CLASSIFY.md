# WF_CLASSIFY - Analyze Request

> **🔍 On step WF_CLASSIFY**

⬆️ OUTPUT THE ABOVE LINE IMMEDIATELY. Do not read further until you have reported your step to the user.

---

## Execute These Steps

1. **Is the request clear?**
   - No → go to WF_CLARIFY
   - Yes → continue

2. **Does this need architectural planning?**
   - New feature, refactor, multi-file design → WF_PLAN_ARCHITECTURE
   - Simple code change, bug fix → WF_DETECT_REQ

3. **Identify affected feature:**
   - Auth, Enquiries, Questions, Audio, Playlists, Profile, Onboarding, Shared

## ⛔ MANDATORY NEXT STEP

**YOU ARE NOT FINISHED.** Before responding to user:

| Condition | MUST Read Next |
|-----------|----------------|
| Request unclear | `WF_CLARIFY` |
| Needs architecture | `WF_PLAN_ARCHITECTURE` |
| Simple change | `WF_DETECT_REQ` |

1. Determine which condition applies
2. Read that WF_* memory NOW
3. Report the new step to user

**SKIPPING THIS TRANSITION = WORKFLOW VIOLATION**

[CRITICAL: Are you on a WF_* workflow step? Did you report on it?]

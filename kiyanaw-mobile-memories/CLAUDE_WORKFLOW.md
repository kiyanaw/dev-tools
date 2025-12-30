# Workflow State Machine

## Overview

Each box is a small memory file (5-15 lines max). Claude reads ONE state at a time.

## Step Reporting

At the start of each workflow step, Claude outputs the icon + step name:
```
**🚀 On step WF_START**
**🔍 On step WF_CLASSIFY**
**⚡ On step WF_EXECUTE**
...etc
```
Icons provide visual distinction and ensure steps aren't skipped.

---

## Main Diagram

```
┌────────────────────────────────────────────────────────────────────────────────────────────────────────────────┐
│                                                   ENTRY                                                        │
│                                                     │                                                          │
│                                                     ▼                                                          │
│                                            ┌───────────────┐                                                   │
│                                            │   WF_START    │                                                   │
│                                            │───────────────│                                                   │
│                                            │ 1. Read       │                                                   │
│                                            │    CLAUDE_    │
│                                            │    OBLIGATIONS│                                                   │
│                                            │ 2. Read       │                                                   │
│                                            │  WORKING_MEM  │                                                   │
│                                            └───────┬───────┘                                                   │
│                                                    │                                                           │
│                                                    ▼                                                           │
│                                            ┌───────────────┐                                                   │
│                                            │  WF_CLASSIFY  │                                                   │
│                                            └───────┬───────┘                                                   │
│                                                    │                                                           │
│                     ┌──────────────┬───────────────┼───────────────┬──────────────┐                            │
│                     │              │               │               │              │                            │
│                     ▼              ▼               ▼               ▼              ▼                            │
│              [continue]    [new feature/     [code/bug]      [research]     [unclear]                          │
│                     │       refactor]             │                │              │                            │
│                     │              │              │                │              │                            │
│                     ▼              ▼              │                ▼              ▼                            │
│            ┌─────────────┐ ┌──────────────┐       │        ┌─────────────┐ ┌─────────────┐                     │
│            │ WF_CONTINUE │ │   WF_PLAN_   │       │        │ WF_RESEARCH │ │ WF_CLARIFY  │                     │
│            │             │ │ ARCHITECTURE │       │        │             │ │             │                     │
│            │ Resume from │ │──────────────│       │        │ Serena only │ │ Ask user,   │                     │
│            │ WORKING_MEM │ │ Spawn arch   │       │        │ No changes  │ │ then return │                     │
│            └──────┬──────┘ │ agent+skill  │       │        └──────┬──────┘ └──────┬──────┘                     │
│                   │        │ Propose plan │       │               │               │                            │
│                   │        └───────┬──────┘       │               │               │                            │
│                   │                │              │               │               │                            │
│                   │         [approved?]           │               │               │                            │
│                   │          │      │             │               │               │                            │
│                   │        [yes]   [no]──►CLARIFY │               │               │                            │
│                   │          │                    │               │               │                            │
│                   │          ▼                    ▼               │               │                            │
│                   │     ┌─────────────────────────────────┐       │               │                            │
│                   │     │        WF_DETECT_REQ            │◄──────┴───────────────┘                            │
│                   │     │─────────────────────────────────│                                                    │
│                   │     │ Scan for requirement language   │                                                    │
│                   │     └────────────────┬────────────────┘                                                    │
│                   │                      │                                                                     │
│                   │              [requirement?]                                                                │
│                   │               │          │                                                                 │
│                   │             [no]       [yes]                                                               │
│                   │               │          │                                                                 │
│                   │               │          ▼                                                                 │
│                   │               │   ┌──────────────────┐                                                     │
│                   │               │   │  WF_REQUIREMENT   │                                                    │
│                   │               │   │──────────────────│                                                     │
│                   │               │   │ 1. Read          │                                                     │
│                   │               │   │    FEATURE_INDEX │                                                     │
│                   │               │   │ 2. Read          │                                                     │
│                   │               │   │    FEATURE_[X]   │                                                     │
│                   │               │   └────────┬─────────┘                                                     │
│                   │               │            │                                                               │
│                   │               │    [new/conflict/exists]                                                   │
│                   │               │      │       │      │                                                      │
│                   │               │    [new] [conflict] │                                                      │
│                   │               │      │       │      │                                                      │
│                   │               │      │       ▼      │                                                      │
│                   │               │      │   CLARIFY    │                                                      │
│                   │               │      │       │      │                                                      │
│                   │               │      ▼       ▼      │                                                      │
│                   │               │  ┌────────────────┐ │                                                      │
│                   │               │  │WF_UPDATE_MEMORY│ │                                                      │
│                   │               │  │────────────────│ │                                                      │
│                   │               │  │ Write/edit     │ │                                                      │
│                   │               │  │ FEATURE_[X]    │ │                                                      │
│                   │               │  └───────┬────────┘ │                                                      │
│                   │               │          │          │                                                      │
│                   │               ▼          ▼          ▼                                                      │
│                   │          ┌────────────────────────────┐                                                    │
│                   │          │      WF_LOAD_FEATURE       │                                                    │
│                   │          │────────────────────────────│                                                    │
│                   │          │ Read FEATURE_INDEX         │                                                    │
│                   │          │ Read FEATURE_[X]           │                                                    │
│                   │          │ Read services-[x] if needed│                                                    │
│                   │          └─────────────┬──────────────┘                                                    │
│                   │                        │                                                                   │
│                   │                        ▼                                                                   │
│                   │          ┌────────────────────────────┐                                                    │
│                   │          │      WF_ARCH_REVIEW        │  ← NEW: Architecture gate                         │
│                   │          │────────────────────────────│                                                    │
│                   │          │ 1. Read ARCH_INDEX         │                                                    │
│                   │          │ 2. Read TP_[LAYER] for each│                                                    │
│                   │          │ 3. Verify compliance       │                                                    │
│                   │          └─────────────┬──────────────┘                                                    │
│                   │                        │                                                                   │
│                   │                [compliant?]                                                                │
│                   │                 │      │                                                                   │
│                   │               [yes]   [no]──►(redesign, loop back)                                        │
│                   │                 │                                                                          │
│                   │                 ▼                                                                          │
│                   │          ┌────────────────────────────┐                                                    │
│                   │          │     WF_ASK_PERMISSION      │                                                    │
│                   │          │────────────────────────────│                                                    │
│                   │          │ Propose with arch justif.  │                                                    │
│                   │          │ "Proceed? (yes/no)"        │                                                    │
│                   │          └─────────────┬──────────────┘                                                    │
│                   │                        │                                                                   │
│                   │                [approved?]                                                                 │
│                   │                 │      │                                                                   │
│                   │               [yes]   [no]──►CLARIFY──►(back to ASK_PERMISSION)                            │
│                   │                 │                                                                          │
│                   │                 ▼                                                                          │
│                   └────────►┌────────────────────────────┐                                                     │
│                             │        WF_EXECUTE          │◄──────────────┐                                     │
│                             │────────────────────────────│               │                                     │
│                             │ Use Serena tools           │               │                                     │
│                             │ Make approved changes only │               │                                     │
│                             └─────────────┬──────────────┘               │                                     │
│                                           │                              │                                     │
│                                           ▼                              │                                     │
│                             ┌────────────────────────────┐               │                                     │
│                             │       WF_CHECKPOINT        │               │                                     │
│                             │────────────────────────────│               │                                     │
│                             │ Update WORKING_MEMORY      │               │                                     │
│                             │ - ✅ completed             │               │                                     │
│                             │ - ⏳ remaining             │               │                                     │
│                             └─────────────┬──────────────┘               │                                     │
│                                           │                              │                                     │
│                                   [more work?]                           │                                     │
│                                    │        │                            │                                     │
│                                  [no]     [yes]──────────────────────────┘                                     │
│                                    │                                                                           │
│                                    ▼                                                                           │
│                             ┌────────────────────────────┐                                                     │
│                             │        WF_VERIFY           │                                                     │
│                             │────────────────────────────│                                                     │
│                             │ 1. Check CLAUDE_OBLIGATIONS│                                                     │
│                             │ 2. Check ARCH_INDEX        │                                                     │
│                             │ 3. Update WORKING_MEM      │                                                     │
│                             │ 4. Update FEATURE_[X]      │                                                     │
│                             └─────────────┬──────────────┘                                                     │
│                                           │                                                                    │
│                                   [violations?]                                                                │
│                                    │        │                                                                  │
│                                  [no]     [yes]────────────────────────────────────────────────────────────────┘
│                                    │              (loop back to START to fix)
│                                    ▼
│                             ┌─────────────┐
│                             │   WF_DONE   │
│                             │─────────────│
│                             │ Summarize   │
│                             │ what was    │
│                             │ done        │
│                             └─────────────┘
└────────────────────────────────────────────────────────────────────────────────────────────────────────────────┘
```

---

## State Descriptions

### WF_START
```
1. Read CLAUDE_OBLIGATIONS memory
2. Read WORKING_MEMORY memory
3. What type of task?
   → New feature/refactor/design: go to CLASSIFY (will route to PLAN_ARCHITECTURE)
   → Code change/bug fix: go to CLASSIFY (will route to DETECT_REQ)
   → Research/question: go to RESEARCH
   → Continue previous: go to CONTINUE
```

### WF_CLASSIFY
```
Analyze user's request:
1. Is it clear what they want?
   → No: go to CLARIFY
   → Yes: continue

2. Does this need architectural planning?
   → New feature, refactor, multi-file design: go to PLAN_ARCHITECTURE
   → Simple code change, bug fix: go to DETECT_REQUIREMENT

3. Which feature area?
   → Identify: auth, audio, enquiries, questions, etc.
```

### WF_PLAN_ARCHITECTURE
```
For tasks requiring design/architecture thinking:

1. Read ARCH_INDEX (overview only, ~50 lines)
2. Determine layers involved
3. Present design to user:
   "Proposed approach:
   - Layers: [service, use-case, hook, etc.]
   - Files: [list]
   - Agent reads: ARCH_SERVICES + TP_SERVICE, etc.

   Approve this design?"

4. Decision:
   → Approved: go to DETECT_REQUIREMENT
   → Not approved: go to CLARIFY (refine design)
```

### WF_DETECT_REQUIREMENT
```
Scan user message for requirement language:
- "should", "must", "needs to", "always", "never"
- Corrections to behavior
- UX preferences

Found requirement?
→ Yes: go to REQUIREMENT
→ No: go to LOAD_FEATURE
```

### WF_REQUIREMENT
```
1. Read FEATURE_INDEX (find which feature memory to check)
2. Read FEATURE_[X] memory
3. Compare user's requirement:
   → NEW: go to UPDATE_MEMORY
   → CONFLICT: go to CLARIFY
   → EXISTS: Acknowledge, go to LOAD_FEATURE
```

### WF_UPDATE_MEMORY
```
1. Identify target memory (FEATURE_[X], ARCH_[LAYER], etc.)
2. Write or edit the memory with new requirement
3. Confirm to user what was updated
→ go to LOAD_FEATURE
```

### WF_CLARIFY
```
ASK USER for clarification:
- "You said X, but memory says Y. Which is correct?"
- "I'm not sure what you want. Do you mean A or B?"
- "You declined the change. What should I do differently?"
- "The proposed design doesn't fit. What constraints am I missing?"

After user responds:
→ Return to previous state with new info
```

### WF_LOAD_FEATURE
```
1. Read FEATURE_INDEX memory
2. Read FEATURE_[X] memory for the affected feature
3. If touching services, read services-[x] memory
4. Note key symbols and files
→ go to ARCH_REVIEW
```

### WF_ARCH_REVIEW
```
Architecture compliance gate (BEFORE proposing changes):
1. Read ARCH_INDEX
2. Identify layers touched (Component, Hook, Use-Case, Service, Store)
3. For each layer, read TP_[LAYER]
4. Verify approach:
   - [ ] Logic in correct layer?
   - [ ] Not bypassing layers?
   - [ ] Business logic NOT in component?

→ Compliant: go to ASK_PERMISSION
→ Not compliant: redesign, loop back
```

### WF_ASK_PERMISSION
```
ASK USER (with architecture justification):
"I plan to modify: [list files/symbols]
Proceed? (yes/no)"

→ Yes: go to EXECUTE
→ No: go to CLARIFY
```

### WF_EXECUTE
```
For multi-layer work, spawn parallel agents:
├── Service agent → ARCH_SERVICES + TP_SERVICE
├── Hook agent → ARCH_HOOKS + TP_HOOK
└── Use-case agent → ARCH_USECASES + TP_USECASE

Each agent:
1. Reads ONLY its ARCH_[LAYER] + TP_[LAYER]
2. Uses Serena tools (find_symbol, get_symbols_overview)
3. Makes approved changes only

After each significant action:
→ go to CHECKPOINT
```

### WF_CHECKPOINT
```
**✅ On step WF_CHECKPOINT**

1. Update WORKING_MEMORY:
   - ✅ What was just completed
   - ⏳ What remains

2. "Significant action" triggers:
   - Created/deleted a file
   - Modified multiple symbols
   - Completed a phase
   - ~5 minutes elapsed

3. Decision:
   → More work remains: go to EXECUTE
   → All work complete: go to VERIFY
```

### WF_VERIFY
```
1. Re-read CLAUDE_OBLIGATIONS - check behavioral violations:
   - [ ] Used `as any`?
   - [ ] Created files without permission?
   - [ ] Guessed paths without Serena?

2. SUB-STEP: Architecture check
   - Read ARCH_INDEX memory
   - Verify changes follow patterns:
     - [ ] Services return ADTs?
     - [ ] Use-cases don't import raw API types?
     - [ ] Correct layer responsibilities?
   - (Future: spawn architecture agent to verify)

3. If violations found → go to START (fix them)

4. Update WORKING_MEMORY with completion

5. Update FEATURE_[X] if architecture changed

→ go to DONE
```

### WF_CONTINUE
```
1. Read WORKING_MEMORY current task
2. Resume from last step
→ go to appropriate state based on where we left off
```

### WF_RESEARCH
```
1. Use Serena tools to explore
2. No code changes allowed
3. Report findings to user
→ go to DETECT_REQ (allows user to continue to implementation)
```

### WF_DONE
```
Task complete.
Summarize what was done.
```

---

## Example Paths

### Path 1: Simple bug fix
```
START → CLASSIFY → DETECT_REQ(no) → LOAD_FEATURE → ARCH_REVIEW → ASK_PERMISSION(yes) → EXECUTE → CHECKPOINT → VERIFY → DONE
```

### Path 2: New feature (needs architecture, multiple actions)
```
START → CLASSIFY → PLAN_ARCHITECTURE → [approved] → DETECT_REQ → LOAD_FEATURE → ARCH_REVIEW → ASK_PERMISSION
  → EXECUTE → CHECKPOINT(more) → EXECUTE → CHECKPOINT(more) → EXECUTE → CHECKPOINT(done) → VERIFY → DONE
```

### Path 3: New feature, design rejected
```
START → CLASSIFY → PLAN_ARCHITECTURE → [user rejects] → CLARIFY → PLAN_ARCHITECTURE → [approved] → ...
```

### Path 4: User gives new requirement
```
START → CLASSIFY → DETECT_REQ(yes) → REQUIREMENT(new) → LOAD_FEATURE → ARCH_REVIEW → ASK_PERMISSION → EXECUTE → VERIFY → DONE
```

### Path 5: Architecture violation in VERIFY
```
... → EXECUTE → VERIFY(arch violation!) → START → CLASSIFY → ... (fix the issue)
```

### Path 6: Research then implement
```
START → RESEARCH → DETECT_REQ → LOAD_FEATURE → ARCH_REVIEW → ASK_PERMISSION → EXECUTE → VERIFY → DONE
```

### Path 7: Continue previous work
```
START → CONTINUE → [resume state] → ... → VERIFY → DONE
```

### Path 8: Architecture violation caught before execution
```
START → CLASSIFY → DETECT_REQ → LOAD_FEATURE → ARCH_REVIEW(violation!) → [redesign] → ARCH_REVIEW(ok) → ASK_PERMISSION → EXECUTE → VERIFY → DONE
```

---

## Memory Files

### Workflow States
| Memory | Purpose |
|--------|---------|
| `WF_START` | Entry point, load obligations |
| `WF_CLASSIFY` | Determine task type, route to arch planning if needed |
| `WF_PLAN_ARCHITECTURE` | Design with architecture agent/skill |
| `WF_DETECT_REQ` | Scan for requirements |
| `WF_REQUIREMENT` | Check FEATURE_INDEX, then FEATURE_[X] |
| `WF_UPDATE_MEMORY` | Write/edit feature memories with requirements |
| `WF_CLARIFY` | Ask user questions |
| `WF_LOAD_FEATURE` | Load feature context |
| `WF_ARCH_REVIEW` | Architecture compliance gate |
| `WF_ASK_PERMISSION` | Get approval for changes |
| `WF_EXECUTE` | Do the work |
| `WF_CHECKPOINT` | Update WORKING_MEMORY, loop or proceed |
| `WF_VERIFY` | Check obligations + architecture |
| `WF_CONTINUE` | Resume previous |
| `WF_RESEARCH` | Research-only path |
| `WF_DONE` | Completion |

### Core Memories
| Memory | Purpose |
|--------|---------|
| `CLAUDE_OBLIGATIONS` | Behavioral constraints only (slim) |
| `ARCH_INDEX` | Architecture overview (5 core concepts) |
| `ARCH_*` | Layer-specific architecture (WHY) |
| `WORKING_MEMORY` | Current task state |
| `FEATURE_INDEX` | Feature → symbol/path lookup |
| `FEATURE_*` | Feature requirements |

### Templates (TP_*)
| Template | Purpose |
|----------|---------|
| `TP_INDEX_LOOKUP` | Pattern → template mapping |
| `TP_WF` | Workflow state template |
| `TP_FEATURE` | Feature memory template |
| `TP_INDEX` | Index file template |
| `TP_SERVICE` | Service layer rules (HOW) |
| `TP_USECASE` | Use-case layer rules (HOW) |
| `TP_HOOK` | Hook layer rules (HOW) |
| `TP_COMPONENT` | Component layer rules (HOW) |
| `TP_STORE` | Store layer rules (HOW) |
| `TP_ADT` | ADT definition rules (HOW) |

### Skills
| Skill | Purpose |
|-------|---------|
| `.claude/skills/architect.md` | Design agent (reads ARCH_INDEX, outputs signatures) |
| `.claude/skills/implementor.md` | Implementation agent (reads ARCH_[LAYER] + TP_[LAYER]) |

---

## Architecture Decision Points

**When to use PLAN_ARCHITECTURE:**
- New feature spanning multiple files
- Refactoring existing code structure
- Adding new service/use-case/hook
- Changing data flow between layers
- User explicitly asks for design/architecture help

**When to skip PLAN_ARCHITECTURE:**
- Bug fix in single file
- Small code change
- Adding to existing pattern (e.g., new field)
- Research/exploration

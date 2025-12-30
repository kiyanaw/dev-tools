# Claude Workflow System - Meta Documentation

This document explains the state machine workflow system used to guide Claude's behavior in this project. Use this to understand, modify, or replicate the approach.

---

## The Problem

Large CLAUDE.md files don't work well:
- Claude skips or forgets instructions buried in long files
- Context fills up, causing drift from instructions
- No enforcement mechanism - rules are suggestions
- Hard to maintain as project evolves

## The Solution: State Machine Workflow

Instead of one large instruction file, we use:
1. **Tiny CLAUDE.md** (~20 lines) - just an entry point
2. **Workflow memories** (WF_*) - small files (~10-15 lines each) that Claude reads one at a time
3. **Each state points to the next** - Claude can't skip ahead because it doesn't know what's next
4. **Verification loops** - violations send Claude back to START

---

## Key Insight

**Claude must read the next instruction file to know what to do next.**

This creates natural enforcement:
- Can't skip steps (doesn't know them yet)
- Can't hallucinate rules (must read from file)
- Must follow the path (each state defines valid transitions)

---

## Core Principle: Context Minimization

**Every file in this system exists because large files cause drift and hallucination.**

- An agent working on a service should NOT read use-case patterns
- An agent working on a hook should NOT read service implementation details
- Load ONLY what's needed for the specific task

This applies to:
- **Workflow files** (WF_*) - small, single-responsibility states
- **Feature files** (FEATURE_*) - user requirements only, not implementation
- **Templates** (TP_*) - layer-specific rules for agents

### Agent Spawning Pattern

When work spans multiple layers:
```
WF_PLAN_ARCHITECTURE:
  1. Read ARCH_INDEX (overview only)
  2. Propose: "Need: service, hook, use-case"
  3. User approves
     ↓
WF_EXECUTE:
  Spawn parallel agents:
  ├── Service agent → ARCH_SERVICES + TP_SERVICE
  ├── Hook agent → ARCH_HOOKS + TP_HOOK
  └── Use-case agent → ARCH_USECASES + TP_USECASE
```

Each agent has minimal, focused context = fewer hallucinations, faster execution.

---

## Step Reporting

Each workflow state includes a reporting line with a distinct icon:

| Step | Report |
|------|--------|
| WF_START | **🚀 On step WF_START** |
| WF_CLASSIFY | **🔍 On step WF_CLASSIFY** |
| WF_PLAN_ARCHITECTURE | **📐 On step WF_PLAN_ARCHITECTURE** |
| WF_DETECT_REQ | **📋 On step WF_DETECT_REQ** |
| WF_REQUIREMENT | **📝 On step WF_REQUIREMENT** |
| WF_UPDATE_MEMORY | **💾 On step WF_UPDATE_MEMORY** |
| WF_CLARIFY | **❓ On step WF_CLARIFY** |
| WF_LOAD_FEATURE | **📂 On step WF_LOAD_FEATURE** |
| WF_ASK_PERMISSION | **🔐 On step WF_ASK_PERMISSION** |
| WF_EXECUTE | **⚡ On step WF_EXECUTE** |
| WF_CHECKPOINT | **✅ On step WF_CHECKPOINT** |
| WF_VERIFY | **🔎 On step WF_VERIFY** |
| WF_CONTINUE | **▶️ On step WF_CONTINUE** |
| WF_RESEARCH | **🔬 On step WF_RESEARCH** |
| WF_DONE | **✨ On step WF_DONE** |

**Benefits:**
- Visual distinction for each step
- User visibility into workflow progress
- Ensures steps aren't silently skipped
- Creates audit trail for debugging

**Implementation:**
- Each WF_* memory starts with its icon + step name
- Claude outputs this line before executing the step

---

## File Structure

```
project/
├── CLAUDE.md                    # Entry point (~20 lines)
│
├── .serena/memories/            # Serena MCP memory storage
│   ├── CLAUDE_META.md           # This file - system documentation
│   ├── CLAUDE_WORKFLOW.md       # Visual diagram of state machine
│   │
│   ├── WF_START.md              # Workflow state
│   ├── WF_CLASSIFY.md           # Workflow state
│   ├── WF_PLAN_ARCHITECTURE.md  # Workflow state
│   ├── WF_DETECT_REQ.md         # Workflow state
│   ├── WF_REQUIREMENT.md        # Workflow state
│   ├── WF_UPDATE_MEMORY.md      # Workflow state (write/edit memories)
│   ├── WF_CLARIFY.md            # Workflow state
│   ├── WF_LOAD_FEATURE.md       # Workflow state
│   ├── WF_ASK_PERMISSION.md     # Workflow state
│   ├── WF_EXECUTE.md            # Workflow state
│   ├── WF_CHECKPOINT.md         # Workflow state (update WORKING_MEMORY)
│   ├── WF_VERIFY.md             # Workflow state
│   ├── WF_CONTINUE.md           # Workflow state
│   ├── WF_RESEARCH.md           # Workflow state
│   ├── WF_DONE.md               # Workflow state
│   │
│   ├── CLAUDE_OBLIGATIONS.md    # Behavioral constraints (never/always)
│   ├── ARCH_INDEX.md            # Architecture overview
│   ├── ARCH_*.md                # Layer-specific architecture
│   ├── WORKING_MEMORY.md        # Current task state
│   │
│   ├── FEATURE_INDEX.md         # Feature → file/symbol lookup
│   ├── FEATURE_*.md             # Feature requirements
│   │
│   ├── TP_*.md                  # Templates for each layer
│   │
│   └── PROJECT_INDEX.md         # Memory navigation
│
└── .claude/skills/              # Claude Code skills
    └── architecture.md          # Architecture agent skill
```

---

## Components Explained

### 1. CLAUDE.md (Entry Point)

The only file Claude reads from disk at conversation start. Must be tiny:

```markdown
# Claude Code - [Project]

## Entry Point

Read `WF_START` memory. Follow the workflow.

## Quick Reference

| Memory | Purpose |
|--------|---------|
| `WF_*` | Workflow states |
| `CLAUDE_OBLIGATIONS` | Hard rules |
| `WORKING_MEMORY` | Current state |
| `FEATURE_*` | Feature requirements |
```

### 2. Workflow States (WF_*)

Small memory files that define:
- What to do in this state
- What to read/check
- Which state(s) to go to next

Example `WF_START`:
```markdown
# WF_START - Entry Point

**Report: "On step WF_START"**

## Execute These Steps

1. **Read CLAUDE_OBLIGATIONS**
2. **Read WORKING_MEMORY**
3. **Classify task type:**
   - Continue previous → `WF_CONTINUE`
   - Research only → `WF_RESEARCH`
   - Code change → `WF_CLASSIFY`

## Next State
Based on classification above.
```

### 3. CLAUDE_OBLIGATIONS.md

Behavioral constraints only. Keep short (~20 lines):

```markdown
# Obligations

## NEVER Do
- [ ] Use `as any` type assertions
- [ ] Create files without permission
- [ ] Guess file paths

## ALWAYS Do
- [ ] Ask before modifying files
- [ ] Use Serena tools before Read/Edit
- [ ] Update WORKING_MEMORY after steps
```

### 4. ARCH_INDEX.md

Architecture overview with 5 core concepts. Layer details in ARCH_[LAYER].

### 5. WORKING_MEMORY.md

Claude's "scratchpad" for current session:
- Current task description
- Progress/status
- Blockers
- Next steps

Claude updates this throughout the workflow.

### 6. FEATURE_*.md and REQS_*.md

Requirements memories describe **what** the app should do, NOT **how**:

**DO** (user story style):
- "Users can filter by their preferred dialect"
- "Filter options come from the languages config"
- "Search results include response counts"

**DON'T** (implementation details):
- "`getActiveLanguages()` returns active languages"
- "Call `searchEnquiries(text, dialect)`"
- Function signatures, parameter types

Implementation details belong in ARCH_* memories or are discovered via Serena tools at execution time. Requirements memories should remain stable even if implementation changes.

Updated when user gives new requirements.

### 7. Skills (.claude/skills/)

Specialized prompts for agents:
- `architect.md` - Design agent (reads ARCH_INDEX, outputs signatures)
- `implementor.md` - Implementation agent (reads assigned ARCH_[LAYER] + TP_[LAYER])

### 8. Templates (TP_*)

Layer-specific rules for context minimization. Each agent reads ONLY its relevant template.

**Workflow templates:**
- `TP_WF` - How to structure workflow states
- `TP_FEATURE` - How to structure feature memories
- `TP_INDEX` - How to structure index files
- `TP_INDEX_LOOKUP` - Pattern → template mapping

**Architecture layer templates:**
- `TP_SERVICE` - Service layer rules
- `TP_USECASE` - Use-case layer rules
- `TP_HOOK` - Hook layer rules
- `TP_COMPONENT` - Component layer rules
- `TP_STORE` - Store layer rules
- `TP_ADT` - ADT definition rules

**Usage:** Before creating a file, check `TP_INDEX_LOOKUP` for the matching template.

---

## Workflow Design Principles

### 1. Small States
Each WF_* file should be 10-20 lines max. If it's longer, split it.

### 2. Explicit Transitions
Every state must declare what states it can transition to:
```
## Next State
- Condition A → `WF_FOO`
- Condition B → `WF_BAR`
```

### 3. Mandatory Checkpoints
Key verification points that can't be skipped:
- `WF_ASK_PERMISSION` - before any code changes
- `WF_VERIFY` - after all code changes
- `WF_CLARIFY` - when uncertain (reachable from multiple states)

### 4. Loop Back on Violations
`WF_VERIFY` checks for violations and loops back to `WF_START` if found. This forces Claude to fix issues rather than ignore them.

### 5. Requirement Detection
`WF_DETECT_REQ` scans every user message for requirement language and routes to `WF_REQUIREMENT` to update feature memories.

---

## Adding New States

1. Create memory file: `WF_NEWSTATE.md`
2. Define: what to do, what to read, next states
3. Update upstream states to route to new state
4. Update `CLAUDE_WORKFLOW` diagram
5. Update `PROJECT_INDEX` memory list

Example:
```markdown
# WF_NEWSTATE - Description

## Execute These Steps

1. Do something
2. Check something

## Next State
- Condition → `WF_NEXT`
```

---

## Modifying Existing States

1. Read current state with `mcp__serena__read_memory`
2. Edit with `mcp__serena__edit_memory`
3. Update diagram if transitions changed
4. Test the workflow path

---

## Setting Up for a New Project

### Prerequisites
- Claude Code CLI
- Serena MCP configured (provides memory tools)

### Steps

1. **Create CLAUDE.md** (entry point)
```markdown
# Claude Code - [Project Name]

## Entry Point
Read `WF_START` memory. Follow the workflow.
```

2. **Create core workflow memories:**
   - `WF_START` - entry, reads obligations
   - `WF_CLASSIFY` - determines task type
   - `WF_CLARIFY` - asks user questions
   - `WF_ASK_PERMISSION` - gets approval
   - `WF_EXECUTE` - does the work
   - `WF_VERIFY` - checks compliance
   - `WF_DONE` - completion

3. **Create constraint memories:**
   - `CLAUDE_OBLIGATIONS` - behavioral rules
   - `WORKING_MEMORY` - current state

4. **Create workflow diagram** in `CLAUDE_WORKFLOW`

5. **Add project-specific states** as needed:
   - Feature-specific memories
   - Architecture rules
   - Skills for agents

---

## Debugging the Workflow

### Claude skipping steps?
- Check if state files are too long (split them)
- Verify transitions are explicit
- Add more checkpoints

### Claude forgetting context?
- Update `WORKING_MEMORY` more frequently
- Break task into smaller states
- Add explicit memory reads in states

### Claude making unauthorized changes?
- Strengthen `WF_ASK_PERMISSION` state
- Add violations to `CLAUDE_OBLIGATIONS`
- Ensure `WF_VERIFY` catches the issue

### Claude hallucinating rules?
- Rule isn't in a memory file - add it
- Memory file too long - Claude skimming
- State transition unclear - make explicit

---

## Memory Tool Reference

Using Serena MCP:

```
# List all memories
mcp__serena__list_memories()

# Read a memory
mcp__serena__read_memory("MEMORY_NAME")

# Write a memory
mcp__serena__write_memory("MEMORY_NAME", "content")

# Edit a memory (regex or literal)
mcp__serena__edit_memory("MEMORY_NAME", "old", "new", "literal")
```

---

## Summary

| Component | Purpose | Size |
|-----------|---------|------|
| CLAUDE.md | Entry point | ~20 lines |
| WF_* | Workflow states | 10-20 lines each |
| CLAUDE_OBLIGATIONS | Behavioral rules | ~20 lines |
| ARCH_INDEX | Architecture overview | ~50 lines |
| WORKING_MEMORY | Session state | Variable |
| FEATURE_* | Feature requirements | Variable |
| TP_* | Templates for agents | 15-25 lines each |
| Skills | Agent prompts | Variable |

**Key principles:**
1. Claude can only know its next step by reading the next memory file
2. Every file is small because large files cause drift and hallucination
3. Agents load ONLY the template relevant to their layer

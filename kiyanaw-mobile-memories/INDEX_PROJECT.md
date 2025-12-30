# INDEX_PROJECT - Memory Navigation

> Quick lookup for finding the right memory.

## Meta (CLAUDE_*)

| Memory | Purpose |
|--------|---------|
| `CLAUDE_META` | System documentation (how workflow works) |
| `CLAUDE_WORKFLOW` | Visual state machine diagram |

## Workflow (WF_*)

| Memory | Purpose |
|--------|---------|
| `WF_START` | Entry point |
| `WF_CLASSIFY` | Determine task type |
| `WF_PLAN_ARCHITECTURE` | Design with ARCH_INDEX |
| `WF_DETECT_REQ` | Scan for requirements |
| `WF_REQUIREMENT` | Handle requirements |
| `WF_UPDATE_MEMORY` | Write/edit memories |
| `WF_CLARIFY` | Ask user questions |
| `WF_LOAD_FEATURE` | Load feature context |
| `WF_ASK_PERMISSION` | Get approval |
| `WF_EXECUTE` | Do the work |
| `WF_CHECKPOINT` | Update WORKING_MEMORY, loop or proceed |
| `WF_VERIFY` | Check compliance |
| `WF_CONTINUE` | Resume previous |
| `WF_RESEARCH` | Research only |
| `WF_DONE` | Completion |

## Templates (TP_*)

| Memory | Purpose |
|--------|---------|
| `TP_INDEX_LOOKUP` | Pattern → template mapping |
| `TP_WF` | Workflow state template |
| `TP_FEATURE` | Feature memory template |
| `TP_INDEX` | Index file template |
| `TP_STYLE` | Style guide template |
| `TP_UI` | Screen documentation template |
| `TP_ARCHITECTURE` | Layer contracts |
| `TP_SERVICE` | Service layer rules |
| `TP_USECASE` | Use-case rules |
| `TP_HOOK` | Hook rules |
| `TP_COMPONENT` | Component rules |
| `TP_STORE` | Store rules |
| `TP_ADT` | ADT rules |

## Architecture (ARCH_*)

| Memory | Purpose |
|--------|---------|
| `ARCH_INDEX` | Overview + core concepts |
| `ARCH_SERVICES` | Service layer (I/O boundary) |
| `ARCH_USECASES` | Use-case layer (orchestration) |
| `ARCH_HOOKS` | Hook layer (pure-callback) |
| `ARCH_COMPONENTS` | Component layer (pure rendering) |
| `ARCH_STORES` | Store layer (ADT state) |
| `ARCH_ADTS` | ADT layer (data boundary) |

## Requirements (REQS_*)

| Memory | Scope |
|--------|-------|
| `INDEX_REQS` | Requirements navigation |
| `REQS_CORE` | App-wide (languages, search, UI patterns) |
| `REQS_PROFILE` | Profile feature requirements |

## Core

| Memory | Purpose |
|--------|---------|
| `CLAUDE_OBLIGATIONS` | Hard rules |
| `WORKING_MEMORY` | Current task |
| `STYLE_GUIDE` | UI/UX conventions |

## Features (FEATURE_*)

| Memory | Feature |
|--------|---------|
| `FEATURE_AUTH` | Authentication |
| `FEATURE_SEARCH_ENQUIRIES` | Browse/Search |
| `FEATURE_ANSWER_QUESTION` | Answer flow |

## Indexes (INDEX_*)

| Memory | Purpose |
|--------|---------|
| `INDEX_GUIDE` | How to use all indexes |
| `INDEX_FEATURE` | Feature → path lookup |
| `INDEX_UI` | Screen → feature mapping |
| `INDEX_REQS` | Requirements lookup |

## Archived (zOLD_*)

Legacy memories preserved for reference. Serena can find this info via symbolic tools.

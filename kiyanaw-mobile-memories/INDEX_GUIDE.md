# INDEX_GUIDE - Finding Your Way

> Check this when you need to find something in the project.

## Index Files

| Index | What It Answers | Check When |
|-------|-----------------|------------|
| `INDEX_PROJECT` | "What memories exist?" | Lost or exploring |
| `INDEX_FEATURE` | "Where's the code/memory for feature X?" | Starting feature work |
| `INDEX_UI` | "What features does screen X use?" | Modifying UI/screens |
| `INDEX_REQS` | "What requirements for feature X?" | Planning features |
| `ARCH_INDEX` | "How do layers connect?" | Architecture decisions |
| `TP_INDEX_LOOKUP` | "What template for this file type?" | Creating new files |

## Quick Patterns

- **Adding a feature** → INDEX_FEATURE → FEATURE_* → INDEX_UI
- **Modifying a screen** → INDEX_UI → FEATURE_* memories it uses
- **Checking requirements** → INDEX_REQS → REQS_CORE + REQS_[FEATURE]
- **Creating files** → TP_INDEX_LOOKUP → TP_* template
- **Architecture question** → ARCH_INDEX → ARCH_* layer details

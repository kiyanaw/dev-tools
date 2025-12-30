# Template Index Lookup

## Pattern → Template Mapping

| Pattern | Template | Purpose |
|---------|----------|---------|
| `app/**/*.tsx` | TP_ROUTE | Route file (view composition) |
| `WF_*.md` | TP_WF | Workflow states |
| `FEATURE_*.md` | TP_FEATURE | Feature requirements |
| `STYLE_*.md` | TP_STYLE | UI/UX conventions |
| `UI_*.md` | TP_UI | Screen documentation |
| `*_INDEX.md` | TP_INDEX | Index/lookup files |
| `TP_*.md` | (this file) | Templates themselves |
| `services/*.ts` | TP_SERVICE | Service layer code |
| `hooks/*.ts` | TP_HOOK | Hook layer code |
| `use-cases/*.ts` | TP_USECASE | Use-case layer code |
| `components/*.tsx` | TP_COMPONENT | Component layer code |
| `stores/*.ts` | TP_STORE | Store layer code |
| `*ADT.ts` | TP_ADT | ADT definitions |

## Usage

Before creating a file:
1. Match pattern above
2. Read the template
3. Follow template structure
4. No template? Ask user to create one

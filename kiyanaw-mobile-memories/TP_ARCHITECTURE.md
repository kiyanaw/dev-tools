# Template: Architecture Overview

## Purpose

Overview + layer contracts only. Each layer has its own TP_[LAYER].md.

## Layer Flow

```
Component → Hook → Use-Case → Service → Store
```

## Contracts (Signatures Only)

| Layer | Receives | Returns |
|-------|----------|---------|
| Service | (params) | `Promise<ADT>` |
| Use-Case | (config via constructor) | `Promise<Result>` via execute() |
| Hook | () | `{ state, callbacks }` |
| Component | (props) | JSX |
| Store | (ADT) | void (updates state) |

## Layer Templates

- TP_SERVICE - Service layer rules
- TP_USECASE - Use-case layer rules
- TP_HOOK - Hook layer rules
- TP_COMPONENT - Component layer rules
- TP_STORE - Store layer rules
- TP_ADT - ADT boundary rules

## When Planning

1. Read this file (contracts)
2. Determine layers involved
3. Spawn agents with layer-specific templates

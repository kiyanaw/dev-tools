# ARCH_INDEX - Architecture Overview

## The WHY: Testability

**The entire point of clean architecture is testability.** Business logic should be testable without React, without APIs, without the UI. If you can't unit test your logic with simple inputs and outputs, the architecture is wrong.

## Core Philosophy

**The UI is dumb.** Components just reflect store state. They don't make decisions, they don't contain logic. They're mirrors.

**Use-cases are like a CLI tool.** A CLI knows nothing about the outside world - you MUST pass in exactly what it needs to operate, including services - what it uses to talk to the outside world. That's what makes it "clean." Use-cases are ignorant of external details and explicit about their inputs.

**Use-cases are stateless.** They run and complete. They don't "sit around" waiting for events. State lives in the store as inert data. Events are managed through services and handled by hooks. When events happen, use-cases are triggered to calculate the next state or perform the next action.

**Dependency flows inward.** Business logic never depends on external details (React, APIs, Zustand).

## Layer Flow

```
Component → Hook → Use-Case → Service → Store
                                  ↓
                                ADT (boundary)
```

| Layer | Role | Philosophy |
|-------|------|------------|
| **Component** | Display state, emit events | "Dumb mirror" - no logic |
| **Hook** | Wire React to use-cases | Thin adapter, zero logic |
| **Use-Case** | THE business logic | Like a CLI - explicit inputs, stateless |
| **Service** | I/O boundary | ONE per feature, all external I/O |
| **Store** | Inert data | State as data, not behavior |
| **ADT** | Domain types | Shape of YOUR domain, not external APIs |

## Five Core Concepts

### 1. Clean Boundary Rule
Services are where the external world enters. This includes the browser, stores, dbs, S3, etc. Services return ADTs, so use-cases NEVER see raw API types. If an API changes, only the service and ADT change - use-cases are protected.

### 2. I/O Isolation
All external I/O happens in Services ONLY. Use-cases orchestrate; they don't fetch. Store access is ALSO I/O - use store services. Use-cases should be pure logic over services.

### 3. Explicit Input Boundary
Use-cases define explicit input types - OUR contract, not external shapes. Never pass `response.json()` or request params directly. Destructure to private vars, not `this.config.foo`.

### 4. ADT as Data Boundary
An ADT wraps raw external data, exposes domain getters. It's "just a dumb object" - no I/O, no side-effects. It represents YOUR domain's shape, not the API's shape.

### 5. State as Data, Events as Triggers
State lives in the store as inert data. When something happens (user action, async completion), a use-case runs, potentially calling a pure function to calculate the next state. The pure function IS the testable business logic.

## Layer Contracts

### Service → Use-Case
```
Service returns: Promise<ADT> or Promise<{ x: ADT, y: ADT[] }>
Use-case receives: ADT instances only (never raw API types)
```

### Store Service → Use-Case
```
Store service: setX(adt: ADT): void, getX(): ADT
Use-case: calls store service functions (never Zustand directly)
```

### Use-Case → Hook
```
Use-case: class with Input interface, execute(): Promise<void>
Hook: useCallback(() => new UseCase(input).execute(), [])
```

### Hook → Component
```
Hook: returns memoized callback
Component: calls hook, reads store via selectors
```

## Layer Reference

| Layer | Memory | Template |
|-------|--------|----------|
| Route | ARCH_ROUTES | TP_ROUTE |
| Component | ARCH_COMPONENTS | TP_COMPONENT |
| Hook | ARCH_HOOKS | TP_HOOK |
| Use-Case | ARCH_USECASES | TP_USECASE |
| Service | ARCH_SERVICES | TP_SERVICE |
| Store | ARCH_STORES | TP_STORE |
| ADT | ARCH_ADTS | TP_ADT |

## Guiding Principles

### The Two Questions
When deciding where logic lives, ask:
1. **Is this easy to test?** - Can I unit test this without mocking the world?
2. **Is this easy to change?** - If requirements shift, can I move this easily?

If both answers are "yes," the current placement is probably fine.

### Simplicity Within Architecture
It's easy to make things more complex. It's harder to make things simpler. Within the architecture constraints, always favor the simpler approach:
- Business logic MUST live in use-cases (testable)
- Hooks ARE adapters (framework bridge)
- I/O happens in services (isolation)

But **complexity has a threshold**. Not everything needs a use-case.

### The Lightweight Pattern
Hooks can shortcut directly to services when:
- The logic is **trivial pass-through** (play/pause, simple toggles)
- There's **no decision-making** that needs testing
- It's **easy to move** to a use-case later if complexity grows

Example of acceptable lightweight:
```typescript
// OK - trivial routing, no business decision
const togglePlayback = useCallback(async () => {
    if (state.isPlaying) await service.pause();
    else await service.play();
}, [state.isPlaying]);
```

Example requiring use-case:
```typescript
// NOT OK - complex decisions belong in use-case
const handleTrackEnd = useCallback(async () => {
    if (repeatMode === 'one') { ... }
    else if (atEnd && repeatMode === 'none') { ... }
    else { ... }  // This belongs in a use-case
}, [repeatMode, atEnd]);
```

**Rule of thumb:** If the if/else is "which service method to call," it's lightweight. If it's "what should happen next," it needs a use-case.

## Acceptable Patterns (Low Priority)

**Cross-feature service imports** - Use-cases or hooks importing services from other features is acceptable. Easy to fix via dependency injection if needed later. Not a blocking violation.

Example: `PlaybackTransitionUseCase` importing `getSignedAudioUrl` from audio feature is fine.

## When to Read ARCH_*

- **WF_PLAN_ARCHITECTURE**: Read ARCH_INDEX for overview
- **Agent execution**: Read only the ARCH_[LAYER] relevant to task

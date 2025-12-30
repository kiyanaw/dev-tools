# ARCH_USECASES - Use-Case Layer

## Philosophy: The CLI Mindset

**Think of a use-case like a CLI tool.** A CLI knows nothing about the outside world - you MUST pass in exactly what it needs. It can't reach out and grab things. That's what makes it "clean."

Use-cases are:
- **Stateless** - Run and complete. They don't "sit around."
- **Explicit** - Input interface IS the contract. No hidden dependencies.
- **Ignorant** - Know nothing about React, Zustand, or API shapes.
- **Testable** - Construct with input, call execute, assert results.

## Two Boundaries

- **Input boundary** - Explicit input types (OUR contract, not external shapes)
- **Output boundary** - Services return ADTs (use-case never sees raw API)

## Core Rules

1. **Explicit input types** - Define interface, destructure to private vars
2. **No config bag** - Never `this.config.foo`, always `this.foo`
3. **validate() method** - Pure validation, may set defaults
4. **execute() method** - Orchestrate via services
5. **NEVER import @/API** - Only knows ADTs
6. **NEVER import Zustand** - Store access via store service

## The Input Boundary Pattern

```typescript
// Explicit input type - OUR contract, not external shape
interface LoadPhraseDetailsInput {
    phraseId: string;
    includeComments?: boolean;
}

export class LoadPhraseDetailsUseCase {
    // Private vars - NOT a config bag
    private readonly phraseId: string;
    private readonly includeComments: boolean;

    constructor(input: LoadPhraseDetailsInput) {
        this.phraseId = input.phraseId;
        this.includeComments = input.includeComments ?? true;
    }

    validate(): void {
        if (!this.phraseId?.trim()) {
            throw new Error('Phrase ID required');
        }
    }

    async execute(): Promise<void> {
        this.validate();
        
        // Store service - use-case doesn't know Zustand
        setEnquiryLoading(true);
        
        // API service - returns ADTs
        const data = await getEnquiryWithResponses(this.phraseId);
        
        // Store service - use-case doesn't know Zustand
        setEnquiryResponses(data.responses);
        setEnquiryLoading(false);
    }
}
```

## Why This Matters

- **Testable** - Construct with input, call execute, assert via store service
- **Explicit** - Input interface IS the contract
- **Decoupled** - Doesn't know Zustand or API shapes

## Anti-Patterns

- ❌ `new UseCase(config)` with `this.config.foo`
- ❌ Passing external response shapes as input
- ❌ `useEnquiryStore.getState()` in use-case
- ❌ Importing from @/API

## Template: TP_USECASE

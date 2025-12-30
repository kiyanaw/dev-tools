# ARCH_HOOKS - Hook Layer

## Philosophy: The Thin Adapter

**Hooks are the thinnest possible bridge between React and business logic.** They exist because React needs hooks for state and callbacks - that's it. They should contain ZERO business logic.

If you find yourself writing `if/else` logic, calculations, or complex state management in a hook, you're doing it wrong. That belongs in a use-case or a pure function.

## Why Hooks Exist

Hooks bridge React to business logic. They provide memoized callbacks that wrap use-cases, keeping React concerns separate from domain logic.

## Core Rules

1. **Zero business logic** - Hooks are thin wrappers
2. **Return memoized callbacks** - useCallback always
3. **No useEffect for data loading** - Call imperatively instead
4. **No local state for loading/errors** - That belongs in store

## The Pure-Callback Pattern

```typescript
export const useLoadPhraseDetails = () => {
    return useCallback(async (phraseId: string) => {
        const useCase = new LoadPhraseDetailsUseCase({ phraseId });
        await useCase.execute();
    }, []);
};
```

## Why This Matters

- **Seam between worlds** - React ↔ pure business logic
- **Framework-agnostic core** - Use-cases can be tested without React
- **Memoization** - Prevents unnecessary re-renders

## The Lightweight Exception

While hooks should generally delegate to use-cases, **trivial pass-through logic** is acceptable:

```typescript
// ACCEPTABLE - trivial routing to service methods
const togglePlayback = useCallback(async () => {
    if (state.isPlaying) await service.pause();
    else await service.play();
}, [state.isPlaying]);
```

This is allowed because:
- **Easy to test** - The service methods are already tested
- **Easy to change** - Can move to use-case later if complexity grows
- **No business decision** - Just "which method to call" routing

### When Lightweight is NOT Acceptable

If the if/else involves business decisions, use a use-case:

```typescript
// NOT ACCEPTABLE - business logic in hook
const handleTrackEnd = useCallback(async () => {
    if (repeatMode === 'one') await service.seek(0);
    else if (atEnd && repeatMode === 'none') await service.stop();
    else await loadNextTrack();  // This is a business decision!
}, [repeatMode, atEnd]);
```

**Rule of thumb:** 
- "Which service method to call" → Lightweight OK
- "What should happen next" → Needs use-case

## Anti-Patterns

- ❌ Complex business logic in hooks
- ❌ Direct API calls
- ❌ useEffect for data loading
- ❌ Local loading/error state
- ❌ Multi-step decisions (use a use-case instead)

## Hook Consolidation Rule

**Prefer FEWER hook files** unless there's clear functional separation.

### Good: One hook file per feature
```
features/learner/hooks/
  useLearner.ts   ← All learner state/handlers
```

### Good: Split only when clearly distinct
```
features/playlists/hooks/
  usePlaylists.ts      ← Core playlist operations
  usePlaylistSync.ts   ← Background sync (separate concern)
```

### Bad: Over-splitting
```
features/learner/hooks/
  useLoadQuestions.ts
  useQuestionState.ts
  useProfileState.ts
  ← Too many files! Consolidate.
```

## Hook Returns Pattern

Hooks return everything components need via destructuring:

```typescript
export function useLearner() {
    // State
    const [activeSegment, setActiveSegment] = useState<'questions' | 'profile'>('questions');
    const [questions, setQuestions] = useState([]);
    
    // Handlers
    const askQuestion = useCallback(() => { ... }, []);
    const saveProfile = useCallback(() => { ... }, []);
    
    // Return flat object for easy destructuring
    return {
        // Segment
        activeSegment,
        setActiveSegment,
        // Questions
        questions,
        questionsLoading,
        refreshQuestions,
        // Profile
        profileData,
        saveProfile,
        // Actions
        askQuestion,
        viewQuestion,
    };
}

// Component usage - take only what you need
const { questions, questionsLoading } = useLearner();
```

## When Unsure

**ASK THE USER** before creating a new hook file:
> "Should this go in the existing `useX` hook or do you want a separate hook?"

## Template: TP_HOOK

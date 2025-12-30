# Template: Hook Layer

## Responsibility

Bridge React to business logic. Pure-callback wrappers.

## Contract

```typescript
// Receives: ()
// Returns: { state, callbacks }

export function useEntity() {
  const entity = useEntityStore(s => s.current);
  
  const create = useCallback((config: CreateConfig) => {
    return new CreateEntityUseCase(config).execute();
  }, []);
  
  const update = useCallback((config: UpdateConfig) => {
    return new UpdateEntityUseCase(config).execute();
  }, []);
  
  return { entity, create, update };
}
```

## Hook Types

1. **Pure-callback** - Wrap use-cases, no effects
2. **State access** - Store selectors only
3. **Adapter** - Effects ONLY for mount/unmount of external libs

## Rules

- **No business logic** - Delegate to use-cases
- **No useEffect for data loading** - Call imperatively
- **useCallback for stability** - Prevent re-renders

## File Location

`src/features/[feature]/hooks/use[Name].ts`

## Checklist

- [ ] Pure-callback pattern?
- [ ] No business logic?
- [ ] No useEffect for loading?
- [ ] Callbacks memoized?

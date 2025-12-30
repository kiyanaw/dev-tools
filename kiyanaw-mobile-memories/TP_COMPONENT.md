# Template: Component Layer

## Responsibility

Pure UI rendering. Call hooks for actions.

## Contract

```typescript
// Receives: props
// Returns: JSX

interface EntityCardProps {
  entity: EntityADT;
  onPress?: () => void;
}

export function EntityCard({ entity, onPress }: EntityCardProps) {
  return (
    <Pressable onPress={onPress}>
      <Text>{entity.name}</Text>
    </Pressable>
  );
}
```

## Rules

- **Pure rendering** - Props in, JSX out
- **No business logic** - Delegate to hooks
- **No direct service calls** - Go through hooks
- **ADT properties only** - Never raw API types

## File Location

`src/features/[feature]/components/[Name].tsx`

## Checklist

- [ ] Pure rendering?
- [ ] No business logic?
- [ ] No direct service calls?
- [ ] Uses ADT properties?

## DO NOT

- Call services directly
- Contain business logic
- Use useEffect for data loading
- Import from @/API

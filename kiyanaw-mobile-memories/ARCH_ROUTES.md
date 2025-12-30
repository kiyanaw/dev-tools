# ARCH_ROUTES - Route File Layer

## Philosophy: Minimal Composition

Route files in `app/` compose which components appear. Components pull their own data from hooks.

**No prop drilling.** Components are self-sufficient.

## The Pattern

```typescript
import { View } from 'react-native';
import { FeatureHeader } from '@/features/[x]/components/FeatureHeader';
import { FeatureContent } from '@/features/[x]/components/FeatureContent';

export default function FeatureRoute() {
    return (
        <View style={{ flex: 1 }}>
            <FeatureHeader />
            <FeatureContent />
        </View>
    );
}
```

## Conditional Rendering

When route needs to switch between components, use minimal hook:

```typescript
import { useFeatureView } from '@/features/[x]/hooks/useFeature';

export default function FeatureRoute() {
    const { activeView } = useFeatureView();
    
    return (
        <View style={{ flex: 1 }}>
            <FeatureHeader />
            {activeView === 'a' ? <ContentA /> : <ContentB />}
        </View>
    );
}
```

## What Route Files CONTAIN

- Component imports from `features/*/components/`
- Minimal hook usage for view switching only
- Layout composition (which components, in what order)
- Simple conditional rendering

## What Route Files DO NOT CONTAIN

- Prop passing to components (components use hooks directly)
- `useState`, `useEffect`, `useCallback`
- Handler definitions
- Helper functions
- Service imports
- StyleSheet definitions
- Type/interface definitions

## Data Flow

```
Store ← Hook ← Component
              ↑
         Route (just composes)
```

Components pull from hooks. Route just decides WHICH components to show.

## Anti-Pattern Detection

If your route file has ANY of these, **STOP and refactor:**
- `useState` → Move to hook
- `useEffect` → Move to hook
- `const handleX = () => {}` → Move to hook
- `function helperFn()` → Move to hook or utils
- `StyleSheet.create()` → Move to component
- Service imports → Move to hook
- Props passed to components → Component should use hook directly

## Where Things Belong

| Thing | Location |
|-------|----------|
| View composition | Route file (`app/`) |
| Reusable UI pieces | `features/[x]/components/` |
| State, handlers, data | `features/[x]/hooks/` |
| Business logic | `features/[x]/use-cases/` |
| Shared styles | `constants/Theme` or feature components |

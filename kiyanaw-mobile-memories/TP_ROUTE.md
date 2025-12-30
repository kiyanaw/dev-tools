# Template: Route File

## Responsibility

View composition only. Assemble components. Components pull their own data from hooks.

## Basic Pattern

```typescript
import { View } from 'react-native';
import { FeatureHeader } from '@/features/[feature]/components/FeatureHeader';
import { FeatureContent } from '@/features/[feature]/components/FeatureContent';

export default function FeatureRoute() {
    return (
        <View style={{ flex: 1 }}>
            <FeatureHeader />
            <FeatureContent />
        </View>
    );
}
```

## With Conditional Rendering

```typescript
import { View } from 'react-native';
import { useFeature } from '@/features/[feature]/hooks/useFeature';
import { FeatureHeader } from '@/features/[feature]/components/FeatureHeader';
import { ContentA } from '@/features/[feature]/components/ContentA';
import { ContentB } from '@/features/[feature]/components/ContentB';

export default function FeatureRoute() {
    const { activeView } = useFeature();
    
    return (
        <View style={{ flex: 1 }}>
            <FeatureHeader />
            {activeView === 'a' ? <ContentA /> : <ContentB />}
        </View>
    );
}
```

## With Route Params

```typescript
import { useLocalSearchParams } from 'expo-router';
import { View } from 'react-native';
import { DetailHeader } from '@/features/[feature]/components/DetailHeader';
import { DetailContent } from '@/features/[feature]/components/DetailContent';
import { DetailProvider } from '@/features/[feature]/providers/DetailProvider';

export default function DetailRoute() {
    const { id } = useLocalSearchParams<{ id: string }>();
    
    // If hook needs ID, wrap in provider or pass to context
    return (
        <DetailProvider id={id}>
            <View style={{ flex: 1 }}>
                <DetailHeader />
                <DetailContent />
            </View>
        </DetailProvider>
    );
}
```

## File Location

- Tab routes: `src/app/(protected)/(tabs)/[name].tsx`
- Stack routes: `src/app/(protected)/[name].tsx`
- Public routes: `src/app/[name].tsx`

## Checklist

- [ ] Under 30 lines?
- [ ] No `useState`, `useEffect`, `useCallback`?
- [ ] No handler definitions?
- [ ] No `StyleSheet.create()`?
- [ ] No service imports?
- [ ] No helper functions?
- [ ] No type/interface definitions?
- [ ] No props passed to components?
- [ ] Only imports components + minimal hooks from features?

## DO NOT

- Define state (`useState`)
- Define effects (`useEffect`)
- Define handlers (`const handleX = () => {}`)
- Define styles (`StyleSheet.create`)
- Import services
- Write helper functions
- Pass props to components (they use hooks)
- Define types/interfaces

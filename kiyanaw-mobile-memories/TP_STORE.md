# Template: Store Layer

## Responsibility

Hold application state. Two parts: store definition + store service.

## Part 1: Store Definition

```typescript
// src/features/[feature]/stores/use[Name]Store.ts

interface EntityState {
  entities: EntityADT[];
  current: EntityADT | null;
  add: (entity: EntityADT) => void;
  setCurrent: (entity: EntityADT | null) => void;
}

export const useEntityStore = create<EntityState>((set) => ({
  entities: [],
  current: null,
  add: (entity) => set((s) => ({ entities: [...s.entities, entity] })),
  setCurrent: (entity) => set({ current: entity }),
}));
```

## Part 2: Store Service

```typescript
// src/features/[feature]/services/entityStoreService.ts

export const setCurrentEntity = (entity: EntityADT) => {
  useEntityStore.getState().setCurrent(entity);
};

export const addEntity = (entity: EntityADT) => {
  useEntityStore.getState().add(entity);
};
```

## Who Uses What

| Layer | Store Definition | Store Service |
|-------|------------------|---------------|
| Component/Hook | ✅ (selectors) | ❌ |
| Use-Case | ❌ | ✅ |

## File Locations

- Store: `src/features/[feature]/stores/use[Name]Store.ts`
- Service: `src/features/[feature]/services/[name]StoreService.ts`

## Checklist

- [ ] Store holds ADT instances?
- [ ] Store service wraps getState()?
- [ ] Use-cases import store service, not store?

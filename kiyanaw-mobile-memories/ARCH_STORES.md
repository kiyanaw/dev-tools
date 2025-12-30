# ARCH_STORES - Store Layer (Zustand)

## Why Stores Exist

Stores hold application state. But use-cases should NOT directly access Zustand - store operations are I/O and belong in store services.

## Two Parts

### 1. Store Definition (Zustand)
Location: `src/features/*/stores/`

```typescript
interface EnquiryState {
    enquiryResponses: PublicResponseADT[];
    loading: boolean;
    error: string | null;
    // setters...
}

export const useEnquiryStore = create<EnquiryState>((set) => ({
    enquiryResponses: [],
    loading: false,
    error: null,
    setEnquiryResponses: (responses) => set({ enquiryResponses: responses }),
    setLoading: (loading) => set({ loading }),
    setError: (error) => set({ error }),
}));
```

### 2. Store Service (I/O Boundary)
Location: `src/features/*/services/[name]StoreService.ts`

```typescript
// Store service wraps Zustand - use-cases call these
export const setEnquiryLoading = (loading: boolean) => {
    useEnquiryStore.getState().setLoading(loading);
};

export const setEnquiryResponses = (responses: PublicResponseADT[]) => {
    useEnquiryStore.getState().setEnquiryResponses(responses);
};
```

## Who Uses What

| Layer | Store Definition | Store Service |
|-------|------------------|---------------|
| Component | ✅ (via hooks) | ❌ |
| Hook | ✅ (selectors) | ❌ |
| Use-Case | ❌ | ✅ |

## Anti-Patterns

- ❌ `useEnquiryStore.getState()` in use-case
- ❌ Raw API types in store
- ❌ Business logic in store

## Template: TP_STORE

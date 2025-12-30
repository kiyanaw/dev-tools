# ARCH_ADTS - Abstract Data Types

## Why ADTs Exist

ADTs map external data to internal domain types. ALL data transformation happens in ONE PLACE. This is THE data transformation boundary.

## Core Rules

1. **Wrap raw API type** - Private field holds raw data
2. **Expose getters** - Computed/derived properties
3. **Handle null/undefined** - Safe defaults
4. **Provide raw access** - When needed by services

## The ADT Pattern

```typescript
export class PublicResponseADT {
    private response: PublicResponse;  // Raw GraphQL type

    constructor(response: PublicResponse) {
        this.response = response;
    }

    // Derived property - extracts dialect from complex field
    get dialect(): string {
        const parts = this.response.languageCommunityIndex.split('_');
        return parts.length > 1 ? parts[1].replace('-dialect', '') : parts[0];
    }

    // Display logic - handles null, sanitization
    get displayName(): string {
        const name = this.response.userName;
        if (!name) return 'Community Member';
        if (name.includes('@')) return name.split('@')[0];
        return name;
    }

    get raw(): PublicResponse { return this.response; }
}
```

## Why This Matters

1. **Schema changes isolated** - Backend renames field? Update ONE getter
2. **Business logic centralized** - Name parsing in ADT, not 47 components
3. **Derived data computed once** - Dialect extraction in one place
4. **Testing straightforward** - Construct ADT, assert properties

## Location
Location: `src/features/shared/core/adt.ts`

## Template: TP_ADT

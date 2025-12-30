# Template: ADT (Abstract Data Type)

## Responsibility

Internal domain representation. Boundary between external and internal.

## Contract

```typescript
export class EntityADT {
  constructor(
    public readonly id: string,
    public readonly name: string,
    public readonly createdAt: Date,
  ) {}
  
  // Factory from API response
  static fromAPI(raw: APIEntity): EntityADT {
    return new EntityADT(
      raw.id,
      raw.name,
      new Date(raw.createdAt),
    );
  }
  
  // Computed properties
  get displayName(): string {
    return this.name.trim() || 'Unnamed';
  }
}
```

## Rules

- **Immutable** - readonly properties
- **fromAPI factory** - Single place for mapping
- **Computed getters** - Derived values
- **No external dependencies** - Pure data

## File Location

`src/features/shared/core/adt.ts` or `src/features/[feature]/types/[Name]ADT.ts`

## Existing ADTs

- UserADT
- PlaylistADT
- PrivateQuestionADT
- PublicResponseADT
- CommentADT

## Checklist

- [ ] Immutable (readonly)?
- [ ] Has fromAPI factory?
- [ ] No external dependencies?
- [ ] Located in correct file?

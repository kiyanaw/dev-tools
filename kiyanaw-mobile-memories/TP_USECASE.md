# Template: Use-Case Layer

## Responsibility

Business logic orchestration. Two boundaries: explicit input, ADT output.

## Contract

```typescript
// Input boundary - OUR contract, not external shape
interface CreateEntityInput {
  name: string;
  ownerId: string;
  notify?: boolean;
}

class CreateEntityUseCase {
  // Private vars - NOT a config bag
  private readonly name: string;
  private readonly ownerId: string;
  private readonly notify: boolean;

  constructor(input: CreateEntityInput) {
    this.name = input.name;
    this.ownerId = input.ownerId;
    this.notify = input.notify ?? true;  // Default in constructor
  }
  
  validate(): void {
    if (!this.name?.trim()) throw new Error('Name required');
  }
  
  async execute(): Promise<EntityADT> {
    this.validate();
    const entity = await createEntity(this.name, this.ownerId);
    setCurrentEntity(entity);  // Store SERVICE, not Zustand
    return entity;
  }
}
```

## Rules

- **Explicit input interface** - Define our contract
- **Private vars, not config bag** - `this.name` not `this.config.name`
- **validate() is PURE** - Zero I/O, may set defaults
- **Store via service** - Never `useStore.getState()`
- **Never import @/API** - Only ADTs

## File Location

`src/features/[feature]/use-cases/[Name]UseCase.ts`

## Checklist

- [ ] Explicit input interface?
- [ ] Private vars (no config bag)?
- [ ] No Zustand imports?
- [ ] No @/API imports?
- [ ] Store access via store service?

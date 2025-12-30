# Template: Service Layer

## Responsibility

I/O boundary. Wrap external APIs, return ADTs.

## Contract

```typescript
// Receives: params
// Returns: Promise<ADT>
export async function getEntity(id: string): Promise<EntityADT> {
  const raw = await client.graphql({ query, variables: { id } });
  return EntityADT.fromAPI(raw.data.getEntity);
}
```

## Rules

- **Stateless** - No internal state
- **Returns ADTs** - Map at the edge
- **Single responsibility** - One service per entity/domain
- **No business logic** - Just I/O

## File Location

`src/features/[feature]/services/[name]Service.ts`

## Checklist

- [ ] Returns ADT, not raw API type?
- [ ] Stateless?
- [ ] Single responsibility?
- [ ] No business logic?

## DO NOT

- Import from stores
- Hold state
- Make business decisions
- Return raw GraphQL types

# ARCH_SERVICES - Service Layer

## Why Services Exist

Services are THE BOUNDARY where external world meets your application. They isolate I/O and transform external data into domain types.

## Core Rules

1. **ONE service per feature** - Each feature has ONE consolidated service file (e.g., `authService.ts`, not `signIn.ts`, `signUp.ts`, `signOut.ts`)
2. **Stateless functions** - No instance state
3. **Perform all I/O** - GraphQL, REST, storage, external APIs
4. **Return ADTs** - Map external data before returning
5. **Use-cases never see raw types** - Mapping happens here

## ONE Service Per Feature Pattern

**WRONG** - Multiple granular service files:
```
features/auth/services/
├── signIn.ts
├── signUp.ts
├── signOut.ts
├── confirmSignup.ts
└── getAuthenticatedUser.ts
```

**RIGHT** - One consolidated service:
```
features/auth/services/
├── authService.ts        # All auth I/O operations
└── authStoreService.ts   # Store access (separate concern)
```

**Why?**
- Easier to find all I/O for a feature
- Clearer dependency graph
- Simpler imports in use-cases
- One place to update when external API changes

## The Boundary Pattern

```typescript
// authService.ts - ALL auth I/O in one place
import { signIn, signUp, signOut, confirmSignUp, getCurrentUser } from 'aws-amplify/auth';
import { SignInResultADT, SignUpResultADT, AuthUserADT } from './authADT';

export const authService = {
    signIn: async (email: string, password: string): Promise<SignInResultADT> => {
        const result = await signIn({ username: email, password });
        return new SignInResultADT(result);
    },

    signUp: async (email: string, password: string): Promise<SignUpResultADT> => {
        const result = await signUp({ username: email, password });
        return new SignUpResultADT(result);
    },

    signOut: async (): Promise<void> => {
        await signOut();
    },

    getCurrentUser: async (): Promise<AuthUserADT | null> => {
        try {
            const user = await getCurrentUser();
            return new AuthUserADT(user);
        } catch {
            return null;
        }
    }
};
```

## Service Types

| Type | Purpose | Example |
|------|---------|---------|
| **Feature Service** | External I/O (APIs, storage) | `authService.ts`, `enquiryService.ts` |
| **Store Service** | Zustand store access | `authStoreService.ts` |

Both are valid services. Feature services handle external I/O. Store services wrap store access for use-cases.

## Why This Matters

- **Use-cases stay pure** - Only know ADTs
- **Schema changes isolated** - Field renamed? Only service + ADT change
- **Replaceable** - Swap GraphQL for REST? Only services change
- **Discoverable** - One file to find all feature I/O

## Anti-Patterns

- ❌ Multiple granular service files per feature
- ❌ Returning raw API types
- ❌ Business logic in services
- ❌ Holding state

## Low Priority (Acceptable)

- Cross-feature service imports - Easy to fix later via dependency injection if needed. Not a blocking violation.

## Template: TP_SERVICE

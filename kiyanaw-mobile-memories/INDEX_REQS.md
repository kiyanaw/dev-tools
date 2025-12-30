# INDEX_REQS - Requirements Navigation

> Lookup for behavioral requirements. REQS vs FEATURE: REQS = "what it must do", FEATURE = "how it's built".

## App-Wide Requirements

| Memory | Scope |
|--------|-------|
| `REQS_CORE` | Languages, user context, search, UI patterns |

## Feature Requirements

| Memory | Feature | Key Requirements |
|--------|---------|------------------|
| `REQS_PROFILE` | Profile | Multi-dialect selection, default dialect |
| `REQS_SEARCH_BROWSE` | Search/Browse | Dialect filtering, profile-based queries |
| `REQS_AUTH` | Auth | (create when needed) |
| `REQS_QUESTIONS` | Questions | (create when needed) |
| `REQS_PLAYLISTS` | Playlists | (create when needed) |

## When to Use

- **WF_PLAN_ARCHITECTURE** → Read INDEX_REQS + relevant REQS_[FEATURE]
- **WF_REQUIREMENT** → Update REQS_[FEATURE] (not FEATURE_*)
- **Adding cross-cutting requirement** → Update REQS_CORE

## REQS vs FEATURE Memories

| Type | Contains | Example |
|------|----------|---------|
| REQS_* | "Must", "should", user preferences, UX rules | "User can select multiple dialects" |
| FEATURE_* | File paths, symbols, implementation notes | "ProfileScreen in src/features/profile/" |

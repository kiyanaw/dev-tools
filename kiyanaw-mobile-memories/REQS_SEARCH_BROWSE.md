# REQS_SEARCH_BROWSE - Search & Browse Requirements

## Filtering

- [ ] Browse/Search filtering is based on user's profile language preferences
- [ ] User's selected dialects (from profile) are sent to OpenSearch
- [ ] No filter chips in UI - filtering is automatic based on profile
- [ ] Frontend displays results returned by OpenSearch without additional filtering
- [ ] OpenSearch matches results where dialect OR responseDialects contains any of user's selected dialects

## Browse Behavior

- [ ] Shows latest questions/answers on load (no user action required)
- [ ] Results filtered by user's selected dialects from profile
- [ ] Infinite scroll to load more
- [ ] Pull-to-refresh for latest

## Search Behavior

- [ ] Shows NOTHING until user performs a search
- [ ] Search results filtered by user's profile dialects
- [ ] Results show: title, userName, responseCount, time ago

## Data Flow

```
Profile (dialects[]) → Use-Case → Service → Lambda → OpenSearch
                                                          ↓
Component ← Store ← Use-Case ← Service ← Lambda ← Results
```

## Response Filtering (Phrase Detail)

- [ ] Responses filtered by user's profile dialects by default
- [ ] Hidden responses count displayed (e.g., "3 Responses (1 hidden)")
- [ ] "Show hidden" toggle to reveal non-matching dialect responses
- [ ] Total response count always shows actual total, not filtered count

## Source of Truth

- User dialects: `UserADT.dialects` (getter returning all selected dialect codes)
- OpenSearch proxy: `amplify/backend/function/mobileosproxy/`

# FEATURE_BROWSE_QUESTIONS - Browse Questions Tab

## User-Facing Behavior

Users can:
- See latest questions/answers on load (no search required)
- Filtered automatically by user's profile dialects (no UI filter)
- Infinite scroll to load more
- Pull-to-refresh for latest
- Tap search bar to open search overlay (reuses Search feature)
- Tap a question to view phrase detail

## Constraints

- Dialect badges (responseDialects) visually muted if they don't match user's profile dialects

- Shows content immediately on load (unlike Search which requires query)
- No filter chips - filtering handled by profile preferences
- All data from OpenSearch via mobileosproxy Lambda
- Results show: text, userName, responseCount, time ago

## Key Files

- `src/app/(protected)/(tabs)/explore.tsx` - Browse screen
- `src/features/enquiries/services/enquiries.ts` - OpenSearch service
- `src/features/enquiries/use-cases/browse-enquiries.ts` - orchestration
- `src/features/enquiries/hooks/useBrowseEnquiries.ts` - UI adapter

## Related Features

- **Search** - Search overlay triggered from this screen
- **Enquiries** - Phrase detail navigation

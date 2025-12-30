# FEATURE_SEARCH_ENQUIRIES - Browse & Search

## User-Facing Behavior

Users can:
- Browse all public questions/enquiries
- Search enquiries by text (full-text search)
- View phrase details with responses and audio
- Filtered automatically by user's profile dialects (no UI filter)
- See real-time updates

## Constraints

- Show NOTHING until user performs a search
- Search results show ONLY: title, userName, responseCount, time ago
- **BANNED**: Curved blue left border on cards
- Frontend cannot access OpenSearch directly - must use mobileosproxy Lambda

## Feature Path

`src/features/enquiries/`

## Related Features

- **Audio** - Audio playback for responses
- **Playlists** - Save enquiries to playlists

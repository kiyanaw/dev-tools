# Feature: Playlists

## Overview
Users can create playlists to organize response recordings for review and practice.

## Architecture (Clean Architecture Pattern)

### Data Flow
```
Component → Hook → Use-Case → Service → Store
                         ↓
                       ADT (boundary)
```

### Layers

| Layer | File | Purpose |
|-------|------|---------|
| ADT | `src/features/shared/core/adt.ts` | PlaylistADT, PlaylistItemADT |
| Store | `src/features/playlists/stores/usePlaylistStore.ts` | Zustand store |
| Store Service | `src/features/playlists/services/playlistStoreService.ts` | Use-case access |
| API Service | `src/features/playlists/services/playlistApiService.ts` | GraphQL ops |
| Use-Cases | `src/features/playlists/use-cases/*.ts` | Business logic |
| Hook | `src/features/playlists/hooks/usePlaylist.ts` | React bridge |

### Use-Cases
- `LoadPlaylistsUseCase` - Fetch all playlists for user
- `LoadPlaylistUseCase` - Fetch single playlist with items
- `CreatePlaylistUseCase` - Create new playlist
- `AddToPlaylistUseCase` - Add response to playlist
- `RemoveFromPlaylistUseCase` - Remove item from playlist
- `DeletePlaylistUseCase` - Delete entire playlist

### GraphQL Models
```graphql
type Playlist @model { id, name, userId, itemCount, items: [PlaylistItem] }
type PlaylistItem @model { id, playlistId, responseId, responseText, enquiryText, audioUrl, languageIndex, addedAt }
```

## UX Requirements

### View Playlist Screen
- **Title Editing Pattern:**
  - Default: Title displays as static text with pencil icon beside it
  - Click pencil: Title becomes editable TextInput, pencil changes to [Save] button
  - Save: Persists change, reverts to static text + pencil

### Playlist Items
- Display response text (h3) and enquiry text (caption)
- Remove button (x icon) to remove items from playlist

## Caching Requirements

### Sync Strategy
- **First load**: Full sync of all playlists + items for user
- **Subsequent loads**: Incremental sync using `lastPlaylistSyncAt` timestamp
- Query `playlistsByUser` with `updatedAt >= lastPlaylistSyncAt`

### Offline Availability
- **CRITICAL**: All playlist audio must be cached locally for offline use
- Download ALL audio files immediately during sync (not lazy)
- Store waveform data locally with audio

### Sync UX
- Show overlay "Syncing playlists" during sync operation
- User should see visual feedback that sync is happening

### Deletion Handling
- When `itemCount` changes (decreased), remove items that no longer exist
- Compare cached items against fetched items to detect deletions
- Remove deleted items from cache immediately

### Response Lookup
- Maintain `responseId → playlistId` map for O(1) lookup
- Build map from cached PlaylistItems after sync
- Enables "is in playlist already" check from any screen

### Storage
- AsyncStorage for playlists, items, sync timestamps
- expo-file-system for audio files
- Local audio path replaces S3 URL for cached files

## Files

### Services
- `playlistCacheService.ts` - AsyncStorage for playlists/items, expo-file-system for audio
- `playlistApiService.ts` - GraphQL ops including incremental sync (`fetchPlaylistsSince`)
- `playlistStoreService.ts` - Store access + response lookup functions

### Components  
- `SyncOverlay.tsx` - Modal showing sync progress

### Hooks
- `usePlaylistSync.ts` - Thin wrapper for SyncPlaylistsUseCase

### Use-Cases
- `SyncPlaylistsUseCase.ts` - Orchestrates full/incremental sync, audio caching, cleanup

### Store Fields (sync-related)
- `isSyncing: boolean` - True during sync
- `syncProgress: { current, total, phase }` - Progress tracking
- `responseLookup: Map<string, string>` - responseId → playlistId map
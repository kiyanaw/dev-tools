# Feature: Player

## Overview
Full-screen audio player for playing playlist content with media controls.

## UX Requirements

### Player Overlay (Dark Theme)
- **Trigger:** Play button on playlist
- **UI:** Full-screen dark overlay (spotify/youtube music style)
- **Display:** Large response text (white), enquiry text (grey) below
- **Controls:**
  - Play/Pause (center, large)
  - Previous/Next (beside play)
  - Progress bar with time indicators
  - X button (top-right) to close
  - Shuffle toggle (bottom)
  - Repeat toggle: none → all → one (bottom)

### Playback Behavior
- Auto-advance to next track when current finishes
- Shuffle randomizes play order
- Repeat modes:
  - none: Stop at end of playlist
  - all: Loop entire playlist
  - one: Loop current track

## Architecture

### Design Pattern: State as Data, Events as Triggers
```
(currentState, event) => nextState
```
The pure reducer function `calculateNextState()` contains ALL playback logic. It's testable without React, without APIs, without the UI.

### Layers

| Layer | File | Responsibility |
|-------|------|----------------|
| **Types** | `core/types.ts` | PlaybackState, PlaybackEvent, RepeatMode |
| **Reducer** | `core/playbackReducer.ts` | Pure transition function - THE business logic |
| **Store Service** | `services/playerStoreService.ts` | Abstracts Zustand access for use-cases |
| **Store** | `stores/playerStore.ts` | Inert state only, no actions |
| **Use-Case** | `use-cases/PlaybackTransitionUseCase.ts` | Orchestrates: reducer → store → audio side effects |
| **Hook** | `hooks/usePlaylistPlayer.ts` | Thin adapter - wires events to use-cases |
| **Component** | `components/PlaylistPlayerOverlay.tsx` | Dumb UI - reads state, emits events |

### Event Flow
```
User Action → Hook callback → PlaybackTransitionUseCase.execute()
                                    ↓
                        calculateNextState(prevState, event)
                                    ↓
                        setPlaybackState(nextState)
                                    ↓
                        Audio side effects (load, play, stop, seek)
```

### PlaybackEvent Types
- `START_PLAYLIST` - Initialize with playlist items
- `TRACK_ENDED` - Auto-advance (respects repeat mode)
- `NEXT_PRESSED` - Manual next (always advances)
- `PREVIOUS_PRESSED` - Go back or restart (3s threshold)

### Navigation Behavior
- **CRITICAL:** Next/Previous only START playback if already playing
- When player is paused/stopped, navigation changes track but does NOT auto-play
- Play button must be pressed explicitly to start playback from idle state
- `TOGGLE_SHUFFLE` - Enable/disable shuffle
- `CYCLE_REPEAT` - none → all → one → none
- `CLOSE` - Hide player

### Test Coverage
- `core/__tests__/playbackReducer.test.ts` - 31 unit tests for pure reducer
- `hooks/__tests__/usePlaylistPlayer.test.ts` - 13 integration tests for use-case

## Lock Screen Controls (Background Playback)

### Configuration
- iOS: `UIBackgroundModes: ["audio"]` in app.json
- Audio mode: `shouldPlayInBackground: true`, `playsInSilentMode: true`

### Lock Screen Metadata
- **Title:** Response text (the phrase being played)
- **Artist:** "kiyânaw" (app name)
- **Artwork:** Optional (none for now)

### Controls
- Play/Pause: ✅ Enabled
- Seek Forward: ✅ Enabled
- Seek Backward: ✅ Enabled

### Implementation (DONE)
- Uses `expo-audio` 1.1.1 (replaced deprecated `expo-av` for playback)
- `AudioPlaybackService.load()` calls `setActiveForLockScreen(true, metadata, options)`
- `AudioPlaybackService.unload()` calls `clearLockScreenControls()`
- Metadata passed from `usePlaylistPlayer` → `useAudioPlayback` → `AudioPlaybackService`

## Technical Requirements

### Audio URL Handling
- **CRITICAL:** Playlist `audioUrls` are S3 paths/keys (e.g., `public/audio/xyz.mp3`), NOT playable URLs
- Must call `getSignedAudioUrl()` from `@/features/audio/services/audioStorage` before loading
- Pattern: `getSignedAudioUrl(path).then(signedUrl => audio.load(signedUrl))`
- Signed URLs are temporary (15 min expiry) - suitable for playback

### AudioPlaybackService Singleton
- **CRITICAL:** Use `getAudioPlaybackService()` to get singleton instance
- All hooks share same service to prevent multiple audio streams
- Service tracks `currentAudioId` - components must compare their audioPath against this to show correct playing state
- Auto-plays on load via `shouldPlay: true` in createAsync
- Uses `loadId` counter to prevent race conditions from concurrent loads
- Only loads when `isVisible=true` to avoid duplicate loads from multiple hook instances

### Error Handling
- All async expo-av operations wrapped in try-catch
- "Seeking interrupted" errors silently caught (non-critical)

## Files
- `src/features/player/stores/playerStore.ts` - Player state
- `src/features/player/hooks/usePlaylistPlayer.ts` - Player hook  
- `src/features/player/components/PlaylistPlayerOverlay.tsx` - Player UI
- `src/features/player/index.ts` - Public exports

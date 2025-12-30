# FEATURE_ANSWER_QUESTION - Mentor Answer Flow

## User-Facing Behavior

Mentors answer learner questions through a 3-step wizard:
1. **Write Response** - Text input for translation/answer
2. **Record Audio** - Voice recording
3. **Confirm & Publish** - Review, edit question text, publish

## Constraints

- Mentor can edit the question text before publishing
- Response text uses `autoCapitalize="none"` (Indigenous language)
- Audio is optional but encouraged
- Back button goes to previous step (not exit)

## Ownership Model

- `PublicQuestion.userId` = **MENTOR** (not learner)
- `PrivateQuestionLinks.userId` = **MENTOR**
- Cached: `publicQuestionText`, `mentorName` on link

## Search Existing Flow

When mentor clicks "Search Existing":
1. **Pre-fill** search bar with the English phrase from the question
2. **Auto-search** on mount - show results immediately
3. **Editable** - user can modify query and re-search
4. **Click to review** - clicking result navigates to detail screen (slide from right)

Uses OpenSearch fuzzy search on `text` field via `mobileosproxy` Lambda.

## Link Queue Flow (Multi-Link)

When mentor clicks a search result:
1. **Slide-in detail screen** - navigates right with standard "< Back" in header
2. **"Link to Question" button** - adds to pending links AND auto-navigates back
3. **Bottom bar button** - single button showing "Review and Submit (N)" with count
4. **Expandable review** - clicking bottom button opens bottom sheet modal
5. **Review panel** - lists queued links with unlink option + comment text input
6. **Submit** - creates all links + adds comment to original question's discussion

### Constraints
- Comment goes to the **private question's** discussion (the learner's original question)
- Navigation uses standard Expo Router stack (< Back in upper left)
- Linking auto-returns to search screen (to allow queuing more)
- Bottom bar is a full-width button, only visible when queue has items

## Feature Path

`src/features/questions/`

## Style Notes

See `STYLE_GUIDE` for:
- Step indicator pattern (dots)
- Waveform colors
- Button styles (primary vs publish)

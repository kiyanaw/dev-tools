# REQS_CORE - App-Wide Requirements

## Purpose
Core requirements ALL features must respect. Check BEFORE feature-specific memories.

---

## Languages & Dialects

**Source of truth:** `src/features/shared/core/languages.ts`

### Requirements
- Language/dialect options are defined in config, never hardcoded in UI
- Only "active" languages appear in user-facing filters and pickers
- Display names come from config (supports future i18n)
- Languages group by "index" (e.g., all Michif dialects together)

### Database Index Formats

| Model | Field | Format | Example |
|-------|-------|--------|---------|
| PublicQuestion | `languageIndex` | `<language>#<dialect>` | `michif#crgn` |
| PublicResponse | `languageCommunityIndex` | `<language>#<dialect>#<community>` | `michif#crgn#general` |

**OpenSearch fields** (via indexing function):
- `language` - Language code (e.g., `michif`)
- `dialect` - Dialect code (e.g., `crgn`)
- `responseDialects[]` - Array of dialect codes from responses

---

## User Context

**Source of truth:** `src/stores/authStore.ts`

### Requirements
- User has a preferred dialect stored in profile
- Mentors have a list of languages they can teach
- Features should default to user's dialect when filtering is available
- Mentor-specific views filter by their teaching languages

---

## Search & Browse

**Source of truth:** `src/features/enquiries/services/enquiries.ts`

### Requirements
- All search/browse queries go through OpenSearch (not AppSync)
- Results include response counts
- Dialect filtering uses the same keys as the languages config
- Services return domain objects, not raw API responses

---

## UI Patterns

**Source of truth:** `STYLE_GUIDE` memory, `src/constants/Theme.ts`

### Requirements
- Filter chips use theme colors
- Empty states have icon + title + subtitle
- Loading states show activity indicator + text

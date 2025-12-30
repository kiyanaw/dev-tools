# OpenSearch Patterns

## Document Structure
OpenSearch documents for questions have these key fields:
- `questionId` - Unique question ID
- `text` - The question text (English)
- `language` - Base language (e.g., "michif")
- `dialect` - Primary dialect code (e.g., "crgh" for Heritage)
- `responseDialects` - Array of dialects with responses (e.g., ["crgh", "crgn"])
- `responseCommunities` - Array of communities
- `responseCount` - Number of responses

## Dialect Codes
- `crgh` - Heritage (Cree Regional Group Heritage)
- `crgn` - Northern
- etc.

## Filtering by Dialect
When searching from the app:
1. `languageIndex` format in app: `"language#dialect"` (e.g., "michif#crgh")
2. Extract dialect: `languageIndex.split('#')[1]` → "crgh"
3. Pass to search API as `language` parameter
4. Backend filters on `responseDialects` array

## API Endpoint
- Service: `mobileosapi`
- Path: `/search`
- Body: `{ query: string, language?: string }`

# INDEX_UI - Screen Overview

> Page → Feature mapping. For symbols/components, see FEATURE_* memories.

## Tab Screens

| Screen | Route | Features |
|--------|-------|----------|
| Home | `/(tabs)/index` | Search, Playlists |
| Browse | `/(tabs)/explore` | Browse Questions, Search |
| Learner | `/(tabs)/learner` | Questions, Profile, Auth |

## Stack Screens

| Screen | Route | Features |
|--------|-------|----------|
| Phrase Detail | `/phrase/[id]` | Enquiries, Audio, Playlists |
| Ask Question | `/ask-question` | Questions |
| Answer Question | `/answer-question` | Questions, Audio |
| Record Voice | `/record-voice` | Audio |
| View Response | `/view-response` | Enquiries, Audio |
| Question Discussion | `/question-discussion` | Questions |
| Edit Question | `/edit-question` | Questions |
| Unanswered | `/unanswered` | Questions |
| Search | `/search` | Search |
| Link Detail | `/link-detail` | Questions |

| View Playlist | `/view-playlist` | Playlists |
| Create Playlist | `/playlist/create` | Playlists |

## Detailed UI Memories

Create UI_[SCREEN] only when needed for complex screens:
- UI_HOME - (if needed)
- UI_BROWSE - (create when implementing)
- UI_PHRASE_DETAIL - (if needed)

## Feature → Screens

| Feature | Primary Screens |
|---------|-----------------|
| Auth | Profile tab, Login (public) |
| Search | Home (overlay), Browse (overlay), /search |
| Browse Questions | Browse tab |
| Enquiries | Phrase Detail, View Response |
| Questions | My Questions, Ask, Answer, Edit, Discussion |
| Audio | Answer Question, Record Voice, Phrase Detail |
| Playlists | Home, Phrase Detail, View Playlist |
| Profile | Profile tab |

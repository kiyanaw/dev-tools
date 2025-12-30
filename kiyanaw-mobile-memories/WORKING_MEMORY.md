# Working Memory - kiyânaw

## Current Task
**COMPLETED**: Learner Dashboard + Architecture Refactor

### Context
Converting "My Questions" tab into "Learner" dashboard with segment control for "My Questions" and "My Profile".

### Mode: Implementation
- [x] Created learner.tsx with segment control UI
- [x] Integrated My Questions content (list, grouping, empty state)
- [x] Integrated My Profile content (avatar, name, dialects, sign out)
- [x] Updated _layout.tsx - new Learner tab, hidden old tabs
- [ ] Verify in app

**Decisions:**
- Single file approach (content inline, like questions.tsx pattern)
- Segment UI with count badge for questions
- "school-outline" icon for Learner tab
- Old files hidden (href: null) rather than deleted for safety

**Files Modified:**
- `src/app/(protected)/(tabs)/learner.tsx` - NEW: Combined dashboard
- `src/app/(protected)/(tabs)/_layout.tsx` - Tab configuration updated

---

## Previous Task
**COMPLETED**: Mute Dialect Badges Feature - Added visual muting to dialect badges on Browse/Search pages when dialects don't match user's profile preferences.

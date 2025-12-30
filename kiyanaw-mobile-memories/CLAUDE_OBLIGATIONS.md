# CLAUDE_OBLIGATIONS - Behavioral Constraints

## NEVER Do

- [ ] Use fallbacks or defensive programming - fail fast instead
- [ ] Synthesized data or "fake" data especially for audio, unless EXPLICITLY asked to do so
- [ ] Cache/duplicate implemented data in memories (point to source files instead)
- [ ] Use `as any` type assertions
- [ ] Create/modify files without user permission
- [ ] Guess file paths (use Serena tools)
- [ ] Run dev servers (user manages these)
- [ ] Run Amplify commands (user manages these)
- [ ] Implement workarounds without asking
- [ ] Proceed when memories conflict with user instructions

## ALWAYS Do

- [ ] Check INDEX_GUIDE when navigating the project
- [ ] Ask before creating or modifying files
- [ ] Use Serena tools before Read/Edit
- [ ] Update WORKING_MEMORY after significant steps
- [ ] Ask for clarification when uncertain

## On Conflicts

If user instruction contradicts memory:
1. STOP
2. ASK for clarification
3. UPDATE memory after confirmation

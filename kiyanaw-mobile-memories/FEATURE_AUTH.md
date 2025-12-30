# FEATURE_AUTH - Authentication

## User-Facing Behavior

Users can:
- Sign in with email/password
- Sign up with email/password
- Confirm email with verification code
- Sign out
- Automatically restore session on app restart

## Constraints

- After signup, user must confirm email before signing in
- Session persists across app restarts
- Sign-out clears all local data
- Users without profile go to onboarding after auth

## Feature Path

`src/features/auth/`

## Related Features

- **Onboarding** - Creates user profile after signup
- **Profile** - Manages user settings

# Privacy & Security

Privacy is a core design principle of the application, but the current production
architecture is more nuanced than a purely offline app. The local core is
offline-first, while AI assistance and shared-list collaboration are optional
network features.

## Local-Core Guarantees

- Core features work without an account.
- Local shopping lists, notes, recipes, meal plans, birthdays, todos, reminders,
  and health entries stay on the device unless the user exports or shares data.
- No ads.
- No sale of user data.
- Backup/export/import is user-controlled.
- Biometric locking can protect sensitive sections such as notes, recipes, meal
  plans, settings, and health tracking.

## Optional Network Features

AI features use a protected backend and require network access. Shared Lists use
Firebase and require Google Sign-In for collaboration. These features are
optional and do not replace the local Room source of truth.

Shared-list cloud data is scoped to collaboration:

- list metadata
- list items
- membership and roles
- invite metadata
- notification preferences
- FCM token metadata
- Google/Firebase profile `photoUrl` values for member avatars

The app does not upload custom avatar files, and member image support is limited
to the authenticated Google/Firebase profile photo URL.

## Analytics Boundary

Firebase Analytics is used as a privacy-first, opt-out usage layer through an
allowlisted wrapper. Analytics events must not contain:

- shopping-list contents
- notes
- recipes
- searches
- health data
- Firebase UID values
- other personal identifiers

## Security Controls

- Firebase App Check for backend protection
- Google Play Integrity nonce flow for AI calls
- `X-Client-Id` participation in abuse protection
- MariaDB-backed rate limiting for AI usage
- Firestore security rules for shared-list reads and limited client writes
- Cloud Functions validation for multi-document and owner-only operations
- hard deletes blocked from shared-list clients
- FCM tokens excluded from local backup and device transfer rules

The security model treats local data, AI traffic, and shared-list collaboration
as separate trust zones.

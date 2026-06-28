# Data Layer Design

The data layer is designed around local reliability, explicit schema evolution,
and clear boundaries between local data and optional cloud features.

## Storage Technologies

- **Room Database** for local user content
- **Kotlin Flow** for reactive local queries
- **Jetpack DataStore** for preferences and lightweight app state
- **Firestore** for optional shared-list collaboration data
- **MariaDB** for backend-side AI rate limiting

## Local Room Model

Room remains the source of truth for the app's local core. Current local data
areas include:

- shopping items
- favorites
- note folders
- notes
- birthdays
- todo items
- health metric entries

The current production schema is version 23. Health tracking is stored as local
structured entries for blood pressure, weight, height, pulse, and custom metrics.
The app does not interpret those values medically or provide diagnosis, warnings,
or treatment guidance.

## Backup Schema

The current backup schema is version 5. It exports local user-owned data and
keeps shared lists as safe cloud references only.

Backup includes local content such as:

- shopping items
- favorites
- folders and notes
- birthdays
- todos
- health metric entries

Backup does not export full shared-list cloud contents, memberships, invite
tokens, FCM tokens, notification rate-limit data, or member profile photo URLs.

## Shared-List Data Boundary

Shared Lists use Firestore as the realtime source of truth. They are intentionally
separate from the local Room lists.

Key Firestore collections include:

- `sharedLists/{listId}`
- `sharedLists/{listId}/members/{uid}`
- `sharedLists/{listId}/items/{itemId}`
- `sharedLists/{listId}/invites/{inviteId}`
- `userSharedLists/{uid}/lists/{listId}`
- `notificationTokens/{uid}/devices/{deviceId}`
- `notificationPreferences/{uid}`

The `userSharedLists` documents are navigation indexes, not authorization
sources. Role checks are based on active membership documents and server-side
validation in Cloud Functions.

## Key Design Decisions

- No destructive Room migrations in production.
- Local data remains usable without Google Sign-In.
- Shared-list creation does not automatically migrate local lists to the cloud.
- Firestore collaboration data is not cached as a second Room source of truth.
- Multi-document shared-list operations are handled by Cloud Functions where
  authorization and index updates must stay consistent.
- Backend AI limits are enforced server-side, not trusted to the Android client.

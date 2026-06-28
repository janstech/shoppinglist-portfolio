# UI & State Management

The UI is implemented with Jetpack Compose and Material 3, with screen state
driven by ViewModels and Kotlin Flow / StateFlow.

## UI Principles

- Declarative UI
- Single source of truth per screen
- State hoisting out of composables where possible
- Clear separation between presentation state, repository calls, and persistence
- Lightweight layouts designed for older and lower-end Android devices

## Main User Flows

- shopping list and favorites
- tasks and local task reminders
- notes and folders
- recipes and meal plans
- birthdays and local birthday reminders
- personal health tracking
- backup/export/import
- settings, theme, language, and biometric lock
- optional AI meal and ingredient flows
- optional shared-list overview and shared-list detail screens

## Compatibility Considerations

The app targets Android 8 (API 26) and newer. UI work is designed with attention
to:

- keyboard / IME behavior
- navigation and status bar insets
- scroll behavior in dialogs and dynamic content screens
- notification permission differences on newer Android versions
- background execution limits for reminder scheduling
- small-screen and landscape usability
- recomposition cost and avoidable layout complexity

## Shared-List UI State

Shared-list screens combine list metadata, member role, active items, and member
summary data from Firestore into a single UI state. The UI exposes actions based
on role:

- owners can invite, rename, delete, clean up, restore, and archive
- editors can add, edit, toggle, and reorder active rows
- viewers are read-only

Realtime item updates and reorder flows are isolated from local Room screens so
that collaboration behavior does not change the offline-first local experience.

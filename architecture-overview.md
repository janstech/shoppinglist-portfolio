# Architecture Overview

Shopping List & Notes is built as an offline-first Android application with
optional online layers. The local core works without an account, while AI and
real-time shared lists are isolated integrations that the user can choose to use.

## Layers

### UI Layer

- Jetpack Compose
- Material 3 components
- screen-level state hoisting
- dialogs, bottom navigation, overflow actions, and App Link entry points
- Android 8+ compatibility considerations for insets, keyboard behavior, small
  screens, permissions, notifications, and background work

### State & Application Logic

- Android ViewModel
- Kotlin Flow / StateFlow
- lifecycle-aware state collection
- explicit UI state models for local screens, AI flows, and shared-list screens

### Local Data Layer

- Room database as the source of truth for local content
- DataStore for lightweight preferences
- repositories and mappers between storage, domain, and UI layers
- explicit migrations and backup schema evolution

### Optional AI Layer

- FastAPI backend as a protected AI gateway
- Firebase App Check verification
- Google Play Integrity nonce flow
- OpenAI integration
- MariaDB-backed daily and burst rate limiting
- server-side AI response validation and sanitization

### Firebase Collaboration Layer

- Firebase Auth with Google Sign-In for shared-list identity
- Firestore for shared-list metadata, members, items, invites, indexes, and
  notification preferences
- Cloud Functions for multi-document operations and owner-only workflows
- FCM for shared-list push notifications
- HTTPS App Links and GitHub Pages fallback for invite links

## Current System Shape

```text
Local core              -> Room + DataStore
AI assistance           -> Android client + FastAPI + App Check + Play Integrity
Shared-list realtime    -> Firebase Auth + Firestore + Cloud Functions + FCM
Usage analytics         -> Firebase Analytics opt-out + allowlisted wrapper
Backup/export/import    -> local file flow + schema v5 cloud references
```

The architecture is optimized for privacy, predictable behavior, and clear
separation between local user-owned data and optional network-dependent features.

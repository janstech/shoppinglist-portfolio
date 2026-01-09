Shopping List & Notes (Kauppalista & Muistiinpanot)
Android Production Architecture Showcase

This repository documents the architecture and design decisions behind Shopping List & Notes, a privacy-first Android application released to the Google Play Store.

This is a portfolio showcase, not a source-code release.

🎯 Project Goals

- Offline-first productivity application (shopping lists + notes)
- Privacy-first by design: no accounts, no analytics, no ads
- Local-first data storage with reliable Room migrations
- Modern Android architecture using Jetpack Compose and Material 3
- Optional AI-assisted meal planning that returns a recipe idea and a shopping-list friendly ingredient list

🧱 Architecture Overview

The application follows a layered architecture:

- UI Layer – Jetpack Compose screens and Material 3 components
- State Layer – ViewModels + Kotlin Flow/StateFlow for predictable state management
- Domain Layer – Use-cases and repository interfaces (business rules separated from UI)
- Data Layer – Room database + DAOs, mappers, and DataStore for settings
- System Integration – Biometric authentication, backup/export/import, and multi-language support (14 languages)

🔐 Privacy & Security

- Core features work fully offline
- No personal accounts and no tracking SDKs
- All user data (lists, favorites, notes, birthdays, settings) is stored locally on the device
- Optional biometric locking for sensitive sections (notes/settings)
- Backup and restore are user-controlled via export/import (share sheet)

🤖 AI Feature (Meal Suggestions)

The AI feature is implemented as an optional assistant:
- The user provides short constraints (quick choices + free text + optional servings)
- The app calls a dedicated backend endpoint to produce one recipe idea and an ingredient list
- Requests are protected with Firebase App Check to reduce abuse
- Rate limits and optional “unlimited” whitelisting are enforced server-side
- Prompt length is intentionally constrained on the client to control costs

📦 Production Status

- ✅ Live on Google Play (Production track)
- ✅ Multi-language UI (14 languages)
- ✅ Lightweight, fast UI with Compose + Material 3
- ✅ Stable release workflow with signed App Bundles (AAB)

ℹ️ Source Code Notice

This repository intentionally does not contain application source code. The full implementation remains private to protect intellectual property and business logic. Architecture descriptions and selective pseudocode snippets may be provided for technical evaluation purposes.

© 2026 Janstech

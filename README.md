Shopping List & Notes (Kauppalista & Muistiinpanot)
Android Production Architecture Showcase

This repository documents the architecture and design decisions behind Shopping List & Notes, a privacy-first Android application successfully released to the Google Play Store.

This is a portfolio showcase, not a source-code release.
🎯 Project Goals

    Offline-first productivity application

    Zero tracking, zero accounts, zero external servers

    Long-term data integrity and safe Room database migrations (v17+)

    Modern Android architecture using Jetpack Compose and Material 3

    Coming Soon: Privacy-focused AI assistant for smart meal planning and shopping suggestions.

🧱 Architecture Overview

The application follows a robust layered architecture:

    UI Layer – Jetpack Compose with reactive Material 3 components

    State Layer – ViewModels + Kotlin Flows for predictable state management

    Data Layer – Room database with complex DAO relations and DataStore for settings

    System Integration – Biometric authentication, automated backups, and multi-language support (12 languages)

Detailed documentation:

    Architecture Overview

    Data Layer Design

    Privacy & Security

    Pseudocode Samples (No Source Code)

🔐 Privacy-First Design

    100% offline operation for core features

    No analytics or ads, respecting user's digital sovereignty

    Biometric Locking for sensitive notes and settings

    User-controlled data with local backup and restore mechanisms

📦 Production Status

    ✅ Production Approved: Officially granted access to Google Play production channel

    ✅ Test Success: 14-day production verification completed with 24 active testers

    ✅ Stability: 0.00% Crash/ANR rate reported via Android Vitals

    ✅ Version: Currently deploying version 2.0.5

ℹ️ Source Code Notice

This repository intentionally does not contain application source code. The full implementation remains private to protect intellectual property and business logic. Architecture descriptions and pseudocode are provided for technical evaluation purposes.

© 2026 Janstech
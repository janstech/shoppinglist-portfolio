Kauppalista & Muistiinpanot (Shopping List & Notes) — Android Production Showcase

This repository showcases the core architecture and data layer of Kauppalista & Muistiinpanot (Shopping List & Notes), a privacy-focused productivity application built for modern Android devices. The application is currently in its final 14-day production verification phase on the Google Play Store after successfully passing the 12-tester alpha milestone.
Privacy-First Philosophy

The core principle of this project is user privacy.

    100% Offline-First: All data is stored locally on the user's device.

    No Tracking: No login is required, and no data is sent to external servers.

    User Control: Users can manually export and import their data through secure backup files.

Technical Highlights
Robust Data Management (Room Persistence)

The application utilizes a complex Room database implementation that ensures data integrity and smooth evolution.

    Schema Migrations: Demonstrates safe migration paths (v16 to v17) to add new features like birthday tracking without data loss.

    Data Integrity: Uses Foreign Keys, indices, and SQLite transactions (withTransaction) for reliable backup and restore operations.

    Reactive DAOs: Leveraging Kotlin Flows to provide real-time updates to the UI whenever the database changes.

Modern UI & Theme Management

Built with Jetpack Compose and Material 3, the app offers a fluid and accessible user experience.

    Dynamic Theme Engine: Implementation of Light, Dark, and System-default themes.

    Reactive Preferences: Uses Jetpack DataStore (instead of SharedPreferences) for modern, thread-safe setting management.

    Material 3 Design: Follows modern Android design guidelines for better accessibility and aesthetics.

Production Features

    Google Voice-to-Text: Allowing users to add products simply by speaking.

    Localization: Full support for 10+ languages, including Finnish, English, Swedish, and German.

Project Status

    12/12 Testers: Milestone reached.

    14-Day Verification: Currently in progress to ensure stability.

    Stability Monitoring: Integrated with Android Vitals, currently reporting zero issues.

Note: This repository contains a sanitized version of the application's architecture to showcase technical skills. The full business logic and UI remains private.

# Kauppalista & Muistiinpanot  
### Android Production Architecture Showcase

This repository documents the architecture and design decisions behind  
**Kauppalista & Muistiinpanot**, a privacy-first Android application currently undergoing Google Play production verification.

This is a **portfolio showcase**, not a source-code release.

---

## 🎯 Project Goals

- Offline-first productivity application
- Zero tracking, zero accounts, zero external servers
- Long-term data integrity and safe schema evolution
- Modern Android architecture and UI practices

---

## 🧱 Architecture Overview

The application follows a layered architecture:

- **UI Layer** – Jetpack Compose + Material 3
- **State Layer** – ViewModels + Kotlin Flows
- **Data Layer** – Room database + DataStore
- **System Integration** – Speech-to-text, backup/export, accessibility

Detailed documentation:
- [Architecture Overview](architecture-overview.md)
- [Data Layer Design](architecture-data-layer.md)
- [UI & State Management](architecture-ui-layer.md)
- [Privacy & Security](architecture-privacy.md)
- [Pseudocode Samples (No Source Code)](architecture-pseudocode.md)


---

## 🔐 Privacy-First Design

- 100% offline operation
- No analytics, no ads, no tracking
- No user accounts or identifiers
- User-controlled local backup and restore

---

## 📦 Production Status

- ✅ Closed Alpha (12/12 testers)
- 🔄 Google Play 14-day production verification
- 📊 Android Vitals: zero issues reported

---

## ℹ️ Source Code Notice

This repository intentionally **does not contain application source code**.

The full implementation remains private to protect:
- Intellectual property
- Business logic
- Security-sensitive mechanisms

Architecture descriptions are provided for **technical evaluation purposes**.

---

© Janstech

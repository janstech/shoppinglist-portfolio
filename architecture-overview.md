# Architecture Overview

The application is built using a clean, modular architecture optimized for
long-term maintainability and safety.

## Layers

### UI Layer
- Jetpack Compose
- Stateless composables where possible
- State hoisted into ViewModels

### State & Logic
- Android ViewModel
- Kotlin StateFlow / Flow
- Lifecycle-aware state collection

### Data Layer
- Room persistence library
- Strict schema versioning
- Explicit migration paths

### System Services
- Speech-to-text via Android RecognizerIntent
- Backup/export using local files
- No cloud dependencies

The architecture is optimized for offline reliability and predictable behavior.

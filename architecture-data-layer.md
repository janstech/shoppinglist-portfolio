# Data Layer Design

The data layer is designed around reliability and schema safety.

## Storage Technologies

- **Room Database**
- **Kotlin Flow**
- **Jetpack DataStore** for preferences

## Key Design Decisions

### Schema Migrations
- Explicit versioned migrations
- No destructive migrations in production
- Forward-compatible schema evolution

### Data Integrity
- Foreign keys and indices
- Transactions for multi-step operations
- Atomic backup and restore flows

### Reactive Data
- DAOs expose Flow-based queries
- UI updates automatically on data changes

This design ensures long-term data safety even as features evolve.

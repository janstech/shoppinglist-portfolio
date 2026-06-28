# Pseudocode Samples (No Source Code)

This document provides implementation-level pseudocode to demonstrate how the
application is structured without publishing the actual production source code.

---

## 1) Local Data Layer: Reactive Items Stream

Goal: the UI receives updates whenever local database content changes.

```text
DAO.observeItemsSorted():
    return database.queryAsFlow(
        "SELECT * FROM shopping_items ORDER BY checked ASC, sortIndex ASC"
    )

Repository.items():
    return DAO.observeItemsSorted()
        .map(row -> mapToDomainModel(row))

ViewModel.state():
    return Repository.items()
        .stateIn(viewModelScope, initialValue = emptyList)
```

Notes:

- UI observes ViewModel state.
- Flow-based queries remove the need for manual refresh logic.
- Ordering is centralized to ensure consistent behavior.

---

## 2) Safe Schema Evolution: Versioned Migrations

Goal: add a local feature without data loss.

```text
Migration(v22 -> v23):
    beginTransaction()
    try:
        execute("CREATE TABLE health_metric_entries (...)")
        execute("CREATE INDEX index_health_metric_entries_type_time ...")
        setTransactionSuccessful()
    finally:
        endTransaction()
```

Notes:

- Migrations are explicit, reviewed, and versioned.
- No destructive migration is used in production.
- New local features are added without reinterpreting older user data.

---

## 3) Backup / Restore: Local Data With Cloud References

Goal: export and import local data safely while avoiding unsafe shared-list
ownership reconstruction.

```text
BackupService.exportToFile(destination):
    localSnapshot = database.transaction {
        shoppingItems = ShoppingDao.getAll()
        favorites = FavoriteDao.getAll()
        notes = NoteDao.getAll()
        folders = FolderDao.getAll()
        birthdays = BirthdayDao.getAll()
        todos = TodoDao.getAll()
        healthEntries = HealthDao.getAll()
    }

    sharedReferences = SharedListRepository.safeCloudReferencesForCurrentUser()

    writeJson(destination, {
        schemaVersion = 5
        payload = localSnapshot + sharedReferences
    })

BackupService.importFromFile(source):
    backup = readJson(source)
    migrated = BackupMigrator.toCurrentSchema(backup)

    database.transaction {
        clearLocalTables()
        insertLocalPayload(migrated.payload)
    }

    if migrated.sharedReferences.matchCurrentFirebaseUser():
        SharedListRepository.refreshFromCloud()
```

Notes:

- Room writes run inside a transaction to avoid partial restores.
- Shared-list references are informational cloud references, not imported
  collaboration data.
- FCM tokens, invites, members, item documents, and profile photo URLs are not
  exported in the backup payload.

---

## 4) Protected AI Request Flow

Goal: call AI features through a server-side trust boundary rather than trusting
the Android client directly.

```text
AndroidClient.requestMealPlan(input):
    appCheckToken = FirebaseAppCheck.getToken()
    integrityNonce = Backend.getIntegrityNonce()
    integrityToken = PlayIntegrity.requestToken(integrityNonce)
    clientId = ClientIdProvider.current()

    return Backend.post("/api/mealplan", input, headers = {
        "X-Firebase-AppCheck" = appCheckToken
        "X-Play-Integrity" = integrityToken
        "X-Client-Id" = clientId
    })

Backend.mealPlan(request):
    verifyAppCheck(request.headers)
    verifyPlayIntegrity(request.headers)
    enforceRateLimit(clientId, scope = "mealplan")
    aiResponse = OpenAI.generate(request.payload)
    return sanitizeAndValidate(aiResponse)
```

Notes:

- App Check and Play Integrity are verified server-side.
- Rate limiting is handled in the backend with MariaDB.
- AI responses are validated before they reach the Android UI.

---

## 5) Shared-List Creation Flow

Goal: create a real-time collaborative list without moving the user's local list
ownership model into the cloud unexpectedly.

```text
Android.createSharedList(localItems):
    ensureGoogleSignIn()
    draft = {
        title = userSelectedTitle
        listType = SHOPPING or TODO
        initialItems = localItems.take(maxCreateItems)
        memberProfile = FirebaseAuth.currentUser.safeDisplayProfile()
    }
    return CloudFunctions.call("createSharedList", draft)

CloudFunction.createSharedList(context, draft):
    requireAuthenticatedGoogleUser(context.auth)
    validateTitle(draft.title)
    validateInitialItems(draft.initialItems)
    validateProfilePhotoUrl(draft.memberProfile.photoUrl)

    transaction {
        create sharedLists/{listId}
        create sharedLists/{listId}/members/{uid} as OWNER
        create sharedLists/{listId}/items for initial active rows
        create userSharedLists/{uid}/lists/{listId} index document
    }
```

Notes:

- Google Sign-In is required only for shared-list collaboration.
- The local list remains local after shared-list creation.
- Cloud Functions own the multi-document write and index creation.

---

## 6) Role-Based Shared-List Actions

Goal: keep collaboration behavior predictable and enforce owner-only cleanup.

```text
SharedListScreen.render(state):
    if state.role == OWNER:
        showInviteRenameDeleteArchiveAndCleanupActions()
    if state.role in [OWNER, EDITOR]:
        showAddEditToggleAndReorderActions()
    if state.role == VIEWER:
        showReadOnlyList()

Repository.archiveCompleted(listId):
    return CloudFunctions.call("archiveCompletedSharedItems", { listId })

CloudFunction.archiveCompletedSharedItems(context, listId):
    requireOwner(context.auth.uid, listId)
    markCompletedActiveItemsAsArchived()
    updateItemCountAndIndexes()
```

Notes:

- Owners can invite, rename, delete, clean up, restore, and archive.
- Editors can add, edit, toggle, and reorder active rows.
- Viewers are read-only.
- Archive and cleanup are owner-only by product decision and server validation.

---

## Repository Notice

These are conceptual outlines designed for technical evaluation. They do not
represent the full application implementation.

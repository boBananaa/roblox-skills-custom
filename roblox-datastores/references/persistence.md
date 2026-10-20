# Persistence protocol and integration

## Model the profile lifecycle

Use the installed library's lifecycle if it supplies one. For custom persistence, explicitly represent these states and who may mutate the record:

| State | Required behavior |
| --- | --- |
| Loading/acquiring | Gameplay and purchases wait or fail safely; no default profile is writable yet. |
| Active owner | Mutations use one owning layer; saves/receipts are ordered and preserve ownership. |
| Closing | Stop accepting new gameplay mutations; coordinate in-flight work and flush within the deadline. |
| Released/lost | Reject writes and stop the old session's mutation path. Do not silently reacquire and replay stale state. |
| Load failed | Do not treat failure as absent data; retry within bounds or end the session safely. |

A local mutex prevents same-server interleaving; it does not establish cross-server ownership. A session lease/lock protocol must atomically acquire ownership, periodically renew before expiration, and check the current ownership token on every write and release. Use a unique session identity, including in Studio where a bare JobId may not be suitable. Plan for process death, lock expiration, rapid rejoin, and old owners still running after a handover. A takeover must fence out old writes rather than merely trusting their timestamps.

Prefer a maintained existing implementation over inventing a lock protocol for a normal gameplay request. Inspect its actual release/receipt API, failure behavior, and documented guarantees.

## Queue and snapshot semantics

All writes to an authoritative profile need one coordination strategy. An autosave holding an older snapshot must not overwrite a newer purchase or gameplay mutation. Queue operations per key, evaluate the appropriate current revision, and only clear dirty state for changes confirmed saved. Do not let an old retry run behind a newer successful save.

If a save is in progress when PlayerRemoving or BindToClose fires, join/drain it through the same owner rather than starting an independent SetAsync loop. Flush different independent profiles concurrently within a bounded deadline, tracking success/failure per profile. A shutdown hook has limited time and cannot guarantee durability during every outage.

## UpdateAsync discipline

An UpdateAsync callback receives the latest backend record and may run again after a conflicting write. It cannot yield. Keep it a deterministic state transformation for the supplied operation: no external API requests, granting Instances, firing remotes, or changing session-wide counters inside it.

Return nil to abort when ownership/schema/operation preconditions fail. Preserve required record fields, metadata, and user IDs according to the data layer's contract. Do not turn absent, malformed, and future-version records into the same empty table.

The UpdateAsync primitive alone is not a full persistence system. Returning a previously cached table from the callback can still overwrite newer state. A pcall failure can have an unknown backend outcome; use idempotent operation identities or reconciliation before repeating a non-idempotent grant.

## Schema and migration

Define supported value types, bounded sizes, finite numbers, schema version, and validation at load. Avoid saving Instances/functions or unbounded event logs. Deeply copy nested defaults per profile; a shared nested default table can connect otherwise unrelated players' state.

Migrations should identify the input version, preserve progress, apply each step once, validate the result, and update the version only after successful transformation. Stop on future schemas. Test current and historical fixtures plus malformed/partial records. Keep a rollback/recovery path appropriate to the destructive change.

A new datastore name creates a new namespace; it does not discover or import old records. If moving keys/stores, coordinate old/new writers, import, verification, cutover, and retention. A leaderboard should be regenerable from the authoritative record; failure to update it must not erase the actual balance.

## Budgets and recovery

Use bounded retries with exponential backoff/jitter for transient failures, aborting errors that cannot improve. Respect current request budgets and per-key throughput, and avoid retry storms. Stagger autosaves and keep the total profile payload bounded. Open Cloud and game servers can share experience-level budget; administrative jobs are part of the capacity plan.

Test in a separate test experience. Enabling Studio API access against a production experience can expose real records, so never treat that checkbox as a harmless local test prerequisite.

## Meaningful failure cases

- A load fails: player cannot earn/spend against defaults that later overwrite saved progress.
- Two saves overlap: the final independent load contains the newest confirmed revision.
- A response is lost after the backend writes: retry does not double-apply the operation.
- An old session resumes after handover: its write is rejected by ownership validation.
- A receipt commits and autosave follows: the later save retains the grant and receipt identity.
- A migration is retried: no duplicate conversion or loss of the original schema's value.
- Shutdown occurs during a retry: flush is bounded, outcomes are reported, and unsaved state is not falsely marked durable.

## Sources

Reviewed 2026-10-20.

- [Best practices](https://create.roblox.com/docs/cloud-services/data-stores/best-practices)
- [Player data and purchasing](https://create.roblox.com/docs/cloud-services/data-stores/player-data-purchasing)
- [GlobalDataStore:UpdateAsync](https://create.roblox.com/docs/reference/engine/classes/GlobalDataStore#UpdateAsync)
- [Error codes and limits](https://create.roblox.com/docs/cloud-services/data-stores/error-codes-and-limits)

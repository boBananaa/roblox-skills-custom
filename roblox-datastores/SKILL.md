---
name: roblox-datastores
description: Design and review Roblox player persistence, profile sessions, autosave, migration, leaderboards, and data-loss recovery. Use for DataStoreService or integration with an existing profile library.
---

# Roblox player data

## Establish ownership before writing

Inspect the schema, key patterns, current library/version, session lifecycle, and every writer, including purchases, autosaves, administrative tools, and Open Cloud jobs. Prefer the existing maintained data layer if it meets the requirements. Do not introduce another independent writer beside a profile library.

Before implementation, propose the record shape, session/queue ownership, migrations, failure behavior, files touched, and validation in an isolated test experience. Wait for approval unless the current instruction approves that concrete plan. Schema design or local implementation does not authorize production-data edits or enabling Studio access to live data.

Read [persistence.md](references/persistence.md) before implementing saves, migrations, session locking, or purchase integration.

## Required behavior

- Distinguish an absent key from a failed load. Only a successfully read absent record may start from defaults. Never save defaults over a record whose load failed.
- Validate stored shape and schema version; distinguish corrupt data from old but migratable data. Preserve a recoverable source before destructive migration.
- Store related profile fields together when they need an atomic update. A leaderboard is a derived view, not the authoritative balance.
- Use server-side persistence for runtime player state. An edit-mode plugin has no normal joined-player session; use plugin settings for tool preferences, place data for authored content, or a deliberately designed authenticated service for external data.
- Serialize operations per profile/key. Coordinate load ownership, gameplay mutation, autosave, receipts, player departure, and shutdown through the same data layer.
- Use UpdateAsync for operations dependent on the latest stored value. A callback may be re-executed and must not yield or perform external effects. Returning a stale session snapshot from UpdateAsync alone does not prevent lost progress.
- Preserve metadata and associated user IDs when the chosen UpdateAsync protocol relies on them. Follow the installed library's documented update contract.
- Wrap service failures, classify retryable errors, and use bounded exponential backoff with jitter. Account for ambiguous write outcomes before retrying non-idempotent operations.
- Stagger periodic saves; coordinate PlayerRemoving and BindToClose with them. Use a bounded flush/drain process and parallelize across independent profiles where appropriate. Do not promise shutdown guarantees or discard unsaved state before a confirmed result.
- Recheck current request/throughput limits for the actual store and access pattern. Avoid hard-coded universal budgets.

## Libraries and migration

Inspect an existing library's maintenance, API, session-lock behavior, conflict handling, shutdown behavior, migration/export path, and purchase guidance. Use its APIs rather than bypassing its lock. Do not install a library based only on popularity or an old example.

Use explicit, versioned, repeat-safe migrations within the chosen data model. Changing a store name or key is a data move requiring a plan to find, import, verify, and retain existing progress; it is not a migration by itself. Refuse to downgrade a record written by a newer schema.

## Verify and report

Exercise missing records, failed loads, malformed/old/future schemas, transient errors, ambiguous writes, overlapping saves, rapid rejoin, lock loss, receipts during saves, and server shutdown as relevant. Use disposable keys in a separate test experience. Verify a save by a later independent load/rejoin, accounting for read caching.

Report the checks actually run and remaining Studio/service integration tests. Prefer terminal and APIs over UI operations. Reuse existing Studio; preserve unsaved work and close any necessary extra session. Keep place backups outside synced script folders and source control.

## Sources

Reviewed 2026-10-20.

- [Data store best practices](https://create.roblox.com/docs/cloud-services/data-stores/best-practices)
- [Player data and purchasing reference](https://create.roblox.com/docs/cloud-services/data-stores/player-data-purchasing)
- [Error codes and limits](https://create.roblox.com/docs/cloud-services/data-stores/error-codes-and-limits)
- [GlobalDataStore](https://create.roblox.com/docs/reference/engine/classes/GlobalDataStore)

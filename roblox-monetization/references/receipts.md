# Durable developer-product fulfillment

## Start with the real data owner

Find existing ProcessReceipt assignments, BindReceiptHandler registrations/filters, product catalog, profile library, autosave, and economy mutation functions. There must be one understood routing design with one coordinated owner for each player's durable state. Do not place an independent coinStore:UpdateAsync beside a profile library that can subsequently overwrite it.

Use the installed library's documented receipt protocol when suitable, reviewing what it actually guarantees. A library name by itself does not establish durable idempotency. For a custom protocol, use Roblox's current player-data/purchasing sample as a reference and adapt it to the project's requirements rather than assuming a tiny callback is production-ready.

## Choose the existing receipt API correctly

| Entry point | Routing | Success / deferred result |
| --- | --- | --- |
| ProcessReceipt | One server-side callback assignment dispatches products. | Enum.ProductPurchaseDecision.PurchaseGranted / NotProcessedYet |
| BindReceiptHandler | DeveloperProduct filtered handlers, plus an optional catch-all. A matching bound handler takes precedence; unmatched receipts can fall through to ProcessReceipt. | Enum.ReceiptDecision.Processed / NotProcessedYet |

Verify the API and ReceiptType support in the target environment before adopting a new path. Do not migrate a working ProcessReceipt integration solely because a newer API exists. For BindReceiptHandler, use Enum.ReceiptType.DeveloperProduct for this workflow; Robux transfer receipt types have different fields/semantics and are outside this developer-product protocol.

Inspect ownership of registrations across startup, hot reload, and teardown. Duplicate product/type registrations or duplicate catch-all registrations can error. BindReceiptHandler returns an RBXScriptConnection that unregisters the handler when disconnected; retain it under the owning service and clean it up deliberately. A NotProcessedYet decision from a matched handler is an unresolved receipt, not a request to invoke the ProcessReceipt fallback too.

## Concrete operation contract

For a currency product, a durable profile might contain a schema version, balance, session ownership, and a purchase-history map. The operation is logically:

```text
fulfill(receipt):
    resolve receipt.ProductId through the server catalog, including retired handlers
    if unrecognized: report and return the active API's deferred decision
    acquire the player's valid data owner or return the deferred decision
    serialize with other mutations/saves for that profile
    atomically transform the latest supported record:
        reject if ownership/schema is invalid
        if PurchaseId is already recorded: preserve record
        otherwise:
            validate reward and resulting bounded balance
            apply the server-defined reward
            record PurchaseId in that same durable commit
    require a confirmed durable result containing this PurchaseId
    reconcile the owning session's state without overwriting newer changes
    return the active API's success decision
```

This is a protocol sketch, not a callable Roblox API or drop-in script. Implement it through the actual persistence layer, including its locking and dirty-revision semantics. The identity is the actual receipt PurchaseId, not a timestamp, ProductId alone, or client-provided token. If history records retain ProductId, verify an existing receipt record is consistent and investigate any mismatch rather than silently reassigning it.

The callback's record transformation cannot yield or grant side effects outside the record. UpdateAsync may invoke it repeatedly. Do not trust a local `didGrant` flag set by a callback invocation as confirmation of backend success. Inspect the confirmed operation result and the data layer's persistence contract.

## Crash and concurrency reasoning

| Boundary | Required outcome |
| --- | --- |
| Failure before commit | Receipt stays unresolved; later processing can retry. |
| Commit succeeds, response lost | A later attempt observes the recorded PurchaseId and does not grant again. |
| Crash after commit, before success acknowledgment | Reprocessing recognizes the committed grant and can acknowledge it. |
| Two servers process one receipt | Ownership and atomic deduplication permit one durable application. |
| Autosave follows receipt | Autosave cannot replace the record with a pre-purchase snapshot. |
| Player leaves during processing | Outcome follows durable state, not whether UI notification succeeded. |
| Unknown/retired product | Unknown stays unresolved; recognized retired products remain fulfillable. |

Recording a receipt in one key and incrementing currency in another creates a crash gap. “Flag first” can lose rewards; “reward first” can duplicate them. If the reward inherently spans records or external effects, design a durable recoverable state machine/outbox with idempotent consumers and explicit completion criteria. Do not claim exactly-once external execution from two ordinary writes.

Temporary effects require a recoverable entitlement, expiry, or redemption protocol. Persisting only a processed flag before spawning a one-time effect is insufficient if a crash prevents the effect and the player has no recovery path. Define when the obligation is fulfilled for that product.

## History growth and operations

Bound record size and monitor receipt history growth. Do not silently prune arbitrary PurchaseIds: an old unresolved receipt could be delivered again. If scale requires archival/compaction, design and test the protocol so it preserves durable duplicate detection. No fixed “keep the last 50” rule is generally safe.

Keep unresolved-product/error reporting useful and bounded. Retain enough information to repair the fulfillment handler without logging credentials or excessive player details. Do not tell the player a purchase was refunded merely because the callback returned NotProcessedYet.

There is no timer-based ProcessReceipt retry guarantee. Under its documented conditions, another purchase or rejoin can cause reprocessing. BindReceiptHandler documents later-opportunity/rejoin redelivery for unresolved developer-product receipts; do not assume a fixed interval. Check the current delivery contract of the API actually used. UI should explain pending fulfillment and reconcile from authoritative state rather than repeatedly prompt the same product.

## Validation scope

Use isolated record fixtures and a service adapter to inject duplicate callbacks, concurrent owners, response loss, failed loads, lost ownership, and stale saves. Check final durable state by a later independent load, not only a local leaderstat. Test the actual integration with the installed profile layer.

Mocks do not validate Marketplace callbacks or real charging. Test supported Roblox purchase flows separately and report exactly which environment/mode was used. A source review is not evidence that real receipts were processed.

## Sources

Reviewed 2026-10-20.

- [MarketplaceService.ProcessReceipt](https://create.roblox.com/docs/reference/engine/classes/MarketplaceService#ProcessReceipt)
- [MarketplaceService.BindReceiptHandler](https://create.roblox.com/docs/reference/engine/classes/MarketplaceService#BindReceiptHandler)
- [Player data and purchasing](https://create.roblox.com/docs/cloud-services/data-stores/player-data-purchasing)
- [Developer products](https://create.roblox.com/docs/production/monetization/developer-products)

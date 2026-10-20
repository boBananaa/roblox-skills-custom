---
name: roblox-monetization
description: Plan and implement Roblox passes, developer products, purchase UI, durable receipt handling, and monetization integrations. Check current official rules for Creator Rewards, Marketplace eligibility, and DevEx when relevant.
---

# Roblox purchases and monetization

## Plan the purchase contract

Inspect product definitions, existing MarketplaceService handlers (both ProcessReceipt and BindReceiptHandler registrations), data/session library, economy mutation paths, and current UI. Identify whether the item is a permanent entitlement, repeatable durable reward, temporary effect, or external Marketplace item; their fulfillment semantics differ.

Before implementation, explain the purchase and failure flow, grant persistence, duplicate handling, UI state, affected files, and test plan. Wait for approval unless the concrete plan is already approved. Reuse the project's data layer and UI components. Implementation approval does not authorize publishing products, changing prices, or spending Robux.

Read [receipts.md](references/receipts.md) for developer products or durable rewards. Do not substitute a short uncoordinated DataStore snippet for integration with an existing player profile.

## Developer products

Use the project's supported server-side receipt entry point. For ProcessReceipt, keep one callback dispatcher rather than assigning competing callbacks. Current documentation also describes BindReceiptHandler with product filters/catch-all routing; a matching bound handler takes precedence over ProcessReceipt. Inspect all routes before editing, preserve the working integration, and verify API availability before choosing a new route. Prompt closure is not proof of successful fulfillment; never grant a developer product through PromptProductPurchaseFinished or a client remote.

Do not mix the return enums: ProcessReceipt uses Enum.ProductPurchaseDecision.PurchaseGranted / NotProcessedYet; BindReceiptHandler uses Enum.ReceiptDecision.Processed / NotProcessedYet. Apply the same durable-fulfillment requirements through either API. See the reference for routing and handler lifetime.

- Use receiptInfo.PurchaseId as the durable deduplication identity. Callbacks can repeat and can run concurrently on different servers.
- Acknowledge success only for a recognized purchase whose reward and deduplication state are durably committed, including a confirmed already-committed purchase, using the active API's success enum.
- For an unknown ProductId or unavailable handler, report it for repair and return the active API's NotProcessedYet value. Keep fulfillment handlers for retired products with pending receipts; disabling sale is not a reason to discard an existing obligation.
- On a load, lock, or persistence failure, keep the receipt unresolved. ProcessReceipt has no automatic timer-based retry guarantee; another purchase or rejoin triggers reprocessing under its documented conditions. Check the selected API's delivery contract rather than inventing a retry interval.
- Coordinate receipts, autosaves, and session state. Independently incrementing a separate store can be overwritten by a later profile save or omit the visible player state.
- A separate receipt flag and balance write are not an atomic transaction. Account for both crash windows; choose a single-record commit or a documented recoverable protocol.

## Passes and presentation

Check pass ownership on the server using UserOwnsGamePassAsync and handle failures separately from a confirmed lack of ownership. Use the documented server-side PromptGamePassPurchaseFinished event for immediate post-purchase perks, with a join-time ownership check for reconciliation. Grant/reapply perks idempotently through the project's entitlement layer.

Clients can display catalog information and prompt purchases, but never decide authoritative ownership or reward amounts. Fetch prices and product information through the supported current API; avoid static price text where regional or personalized pricing applies. Distinguish canceled, pending fulfillment, successful grant, and failed UI states. A notification is a refresh hint, not the durable record.

For paid random items or restricted products, consult current Roblox policy and applicable PolicyService eligibility before proposing implementation. Do not assume every player or region can access the same products.

## Business guidance

Look up current official rules when discussing payouts, fees, eligibility, or prices. Creator Rewards supersedes the old engagement-based Premium Payouts model. Do not require a Premium subscription for DevEx based on old tutorials, or quote a universal DevEx rate without checking current earning-date rules. Check Marketplace eligibility and price floors for the exact item category rather than assuming a 1 Robux minimum or fixed revenue split.

These are runtime game-purchase workflows. Selling a Studio plugin through the Creator Store is a separate product/distribution workflow with separate terms.

## Verify

Test duplicate, concurrent, unknown, retired, and failed receipts; missing profiles; departure during fulfillment; crash before/after commit; later autosave; and UI reconnection. Use an isolated test experience and the documented test modes. Mock receipts establish application behavior, not a real completed Marketplace transaction. Report that distinction.

## Sources

Reviewed 2026-10-20; consult current business and policy sources when answering those questions.

- [ProcessReceipt contract](https://create.roblox.com/docs/reference/engine/classes/MarketplaceService#ProcessReceipt)
- [BindReceiptHandler contract](https://create.roblox.com/docs/reference/engine/classes/MarketplaceService#BindReceiptHandler)
- [Developer products](https://create.roblox.com/docs/production/monetization/developer-products)
- [Player data and purchasing](https://create.roblox.com/docs/cloud-services/data-stores/player-data-purchasing)
- [Passes](https://create.roblox.com/docs/production/monetization/passes)
- [Creator Rewards](https://create.roblox.com/docs/creator-rewards)
- [DevEx requirements](https://en.help.roblox.com/hc/en-us/articles/203314100-Developer-Exchange-DevEx-Overview-How-to-Submit-Requirements)
- [PolicyService](https://create.roblox.com/docs/reference/engine/classes/PolicyService)

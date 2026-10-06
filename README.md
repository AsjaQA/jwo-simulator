# JWO Store Simulator

Browser-based simulator for testing the RetailCloud **Just Walk Out (JWO)** integration.

**Live:** https://asjaqa.github.io/jwo-simulator/

## Flow

1. **Login** — username, password and domain → access token (`POST /goauth/oauth/token`). The token stays in the browser tab only.
2. **Gates** — up to eight gates run at the same time, each with its own shopper, customer, basket, trip ID and live cart. "Walk out all" sends every gate's order at once.
3. **Customer identity (optional)** — scan the shopper's QR with the camera (or upload a photo of it, or type the code) and the gate calls identify (`POST /integrations/external/orders/customers/identify`); the customer number from the response is used for the order. Or enter a customer number directly and skip identify.
4. **Live cart** — each gate mirrors its basket into the RetailCloud cart API (`/console/transactions/carts`) and shows the priced cart (names, prices, tax, total), refreshed every few seconds. Display only; the cart is abandoned when the trip ends.
5. **Walk out** — creates the order (`POST /integrations/external/orders/sync`) with a unique `idempotentShoppingTripId`.
6. **Resync** — retry failed orders (`POST /integrations/external/orders/retry`) and set up the automatic retry job.

Also: JWO connection check (fills the JWO store ID and the RC store used for the cart `Entitlement` header), item sync, and scheduler jobs (`ECommerceItemSyncJob`, `ExternalOrderTriggerJob`).

Environments: Beta (`dev-platform.rc.fyi`), UAT, Prod. Camera scanning needs HTTPS (GitHub Pages) or localhost.

## Run locally

```bash
python3 -m http.server 4545
```

Then open http://localhost:4545.

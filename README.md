# JWO Store Simulator

Browser-based simulator for testing the RetailCloud **Just Walk Out (JWO)** integration.

**Live:** https://asjaqa.github.io/jwo-simulator/

## Flow

1. **Login** — username, password and domain → access token (`POST /goauth/oauth/token`). The token stays in the browser tab only.
2. **Customer identity** — the shopper gives their code at the entry gate (`POST /integrations/external/orders/customers/identify`). The customer number from the response is used for the order.
3. **Shopping** — pick items from the shelf into the basket.
4. **Walk out** — creates the order (`POST /integrations/external/orders/sync`).
5. **Resync** — retry failed orders (`POST /integrations/external/orders/retry`) and set up the automatic retry job.

Also: JWO connection check, item sync, and scheduler jobs (`ECommerceItemSyncJob`, `ExternalOrderTriggerJob`).

Environments: Beta (`dev-platform.rc.fyi`), UAT, Prod.

## Run locally

```bash
python3 -m http.server 4545
```

Then open http://localhost:4545.

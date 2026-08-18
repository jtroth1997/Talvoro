# Talvoro platform architecture

Talvoro is intended to become a multi-supplier commerce platform rather than a conventional single-supplier dropshipping store. The customer should experience one retailer, one catalogue, one checkout, one account and one order-tracking experience. Supplier complexity stays behind the platform.

## Core principles

1. **Supplier-agnostic catalogue** — every supplier connector maps source data into Talvoro's standard product, variant, inventory, fulfilment and tracking models.
2. **Live commerce data** — supplier cost, stock and delivery estimates are synchronised regularly and revalidated before an order is accepted.
3. **Landed-cost pricing** — selling price is calculated from product cost, shipping, payment cost, expected returns/risk allowance, tax treatment and target margin rather than a fixed imported price.
4. **Automatic fulfilment** — paid orders are routed to the chosen supplier automatically; supplier order IDs and tracking flow back into Talvoro.
5. **UK-first routing** — where commercially sensible, prefer UK inventory, then suitable EU inventory, then global inventory based on total landed cost, delivery speed and supplier reliability.
6. **One product, multiple sources** — duplicate supplier listings should eventually be matched into a canonical Talvoro product. Customers see one listing while the routing engine can choose the best source.
7. **Retailer responsibility** — Talvoro remains responsible for the customer experience, refunds, returns, disputes, product compliance and support even when fulfilment is performed by a supplier.

## Proposed services

### Storefront
Responsive customer web app for discovery, search, product pages, basket, checkout, account, saved products, orders, returns and tracking.

### Product catalogue
Canonical `products` and `variants` tables separate customer-facing product information from supplier offers. Product matching/deduplication can evolve from GTIN/brand/MPN matching to similarity-based matching.

### Supplier connectors
Each supplier integration implements a common interface:
- catalogue import/update
- variant mapping
- stock query
- supplier price query
- shipping quote/estimate
- create order
- confirm/pay order where supported
- order status
- tracking
- cancellation/refund capabilities where supported

Initial integrations should prove the model with one supplier before adding a second. CJdropshipping is a logical first API candidate; the architecture must not depend on CJ-specific fields.

### Offer and routing engine
A product can have many supplier offers. Rank eligible offers using factors such as:
- landed cost
- stock confidence
- warehouse/country
- delivery estimate
- supplier success/cancellation rate
- return handling
- recent API reliability

Never route solely on unit price.

### Pricing engine
Illustrative configurable rules, not final commercial policy:
- lower-value goods may require a higher percentage margin
- higher-value goods may use a lower percentage margin with a minimum cash contribution
- prices should include buffers for payment processing, returns, currency movement and fulfilment variance
- supplier cost changes trigger recalculation
- material price changes should invalidate stale baskets or require explicit revalidation at checkout

### Order orchestration
1. Customer submits checkout.
2. Payment is authorised.
3. Revalidate supplier stock, landed cost and delivery promise.
4. Select best eligible supplier offer.
5. Create/confirm supplier order.
6. Capture/settle customer payment according to final payment design.
7. Store supplier order reference.
8. Pull/webhook fulfilment and tracking updates.
9. Present a single Talvoro order timeline to the customer.
10. Exceptions enter an operations queue rather than silently failing.

### Operations/admin
Internal dashboard should eventually cover:
- supplier/API health
- sync failures
- low margin/negative margin products
- stock conflicts
- unfulfilled orders
- supplier cancellations
- late orders
- refunds/returns
- chargebacks
- product compliance flags
- prohibited/restricted product controls
- manual product suppression
- supplier performance

## Suggested data model

- `products`
- `product_variants`
- `product_media`
- `categories`
- `suppliers`
- `supplier_offers`
- `supplier_inventory`
- `supplier_sync_runs`
- `customers`
- `addresses`
- `baskets`
- `basket_items`
- `orders`
- `order_items`
- `fulfilments`
- `tracking_events`
- `payments`
- `refunds`
- `returns`
- `pricing_rules`
- `supplier_health`
- `product_compliance`

## Scale strategy

Do not launch by blindly publishing every supplier SKU. The platform can be engineered for tens of millions of offers while the customer catalogue remains curated. Poor listings, duplicates, restricted products, weak imagery and unreliable fulfilment should be suppressed.

### Phase 1
One supplier, curated catalogue, real search, product pages, basket, checkout, stock/price sync, automatic order creation and tracking.

### Phase 2
Second supplier. Prove the normalised connector model and multi-source product architecture.

### Phase 3
Canonical product matching and automated supplier selection.

### Phase 4
Add UK/EU/global suppliers, better routing, returns tooling, compliance controls and supplier performance scoring.

### Phase 5
Millions of customer-ready products, advanced search/recommendations, mature routing and high automation.

## Important launch work

Before accepting real customer orders, Talvoro will need production payment processing, tax/VAT design, consumer terms, privacy/cookie compliance, returns/refund policy, product safety/compliance controls, restricted product filtering, customer support processes, fraud controls, supplier contracts and monitoring/alerting.

The current repository storefront is a front-end prototype. Product prices, reviews, delivery estimates and inventory shown there are demonstration data only until connected to production commerce services.
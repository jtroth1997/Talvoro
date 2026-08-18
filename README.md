# TALVORO

**Everything. One place.**

Talvoro is an early-stage marketplace project designed around a simple customer proposition: a huge range of products, one search, one checkout and one place to manage orders.

This repository currently contains the first responsive storefront prototype and the initial platform architecture.

## Current prototype

- responsive desktop/mobile storefront
- marketplace search interaction
- category discovery
- demonstration product catalogue
- saved-product and basket interactions
- trust, delivery and tracking messaging
- scalable platform plan in `ARCHITECTURE.md`

> Product information, prices, reviews, stock and delivery estimates in the prototype are demonstration content only.

## Direction

The production platform is intended to connect multiple supplier APIs behind a normalised Talvoro catalogue, keep stock and cost data current, calculate safe selling prices, automatically route orders and return fulfilment/tracking information to customers.

The customer-facing brand should remain supplier-agnostic. Supplier and dropshipping mechanics belong in the platform layer, not the shopping experience.

## Next build stages

1. Production application stack and database.
2. Customer accounts and authentication.
3. Real catalogue/search schema.
4. First supplier connector.
5. Price and inventory synchronisation.
6. Basket and production checkout.
7. Automated order orchestration.
8. Tracking and customer order centre.
9. Admin/operations dashboard.
10. Second supplier connector and automated offer routing.

See `ARCHITECTURE.md` for the broader system design.
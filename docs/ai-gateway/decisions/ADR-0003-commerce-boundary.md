# ADR-0003: Keep AI gateway and digital-goods commerce responsibilities separate

- **Status:** Proposed — confirm against the final OxyChain platform boundary
- **Date:** 2026-11-25
- **Scope:** Service ownership and integration

## Context

The broader product direction includes AI models as well as non-AI digital products such as eSIMs and gift cards. The AI gateway's core responsibilities are provider access, model routing, protocol compatibility, usage, and AI-specific account operations. Commerce products require catalogues, offers, inventory, orders, payment, fulfilment, and lifecycle management.

## Proposed decision

Keep this repository focused on AI execution and gateway operations. Keep non-AI digital product catalogue, checkout, payment, orders, inventory, and fulfilment in the commerce platform (Medusa is the planned commerce authority). Integrate through authenticated, versioned service APIs rather than sharing database tables or importing commerce domain models into the gateway.

## Alternatives

- **One gateway owns every digital product:** rejected because it couples unrelated provider lifecycles and makes the AI gateway responsible for commerce state.
- **Commerce platform owns AI protocol routing:** rejected because model protocol conversion, provider credential handling, and AI-specific routing belong in the gateway.
- **Separate services with explicit APIs:** proposed because ownership remains clear and services can evolve independently.

## Consequences

- This repository does not implement eSIM or gift-card provider adapters.
- AI product entitlement and usage reconciliation need explicit API contracts.
- Commerce owns order/payment state; gateway owns AI request execution and AI usage evidence.
- No direct database coupling between Medusa and this repository.

## Open questions

- Exact entitlement/provisioning API.
- Webhook authentication and replay protection.
- Usage reconciliation and refund semantics.
- Whether AI product catalogue/pricing is mastered by commerce or synchronized from the gateway.

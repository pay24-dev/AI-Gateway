# AI Gateway architecture rules

Read this file before product, domain-model, architecture, cross-cutting, or data-model work.

## Product boundary

- This repository owns the AI API gateway domain: upstream AI providers, provider connections/accounts, model routing, API protocol handling, usage, cost, access control, and AI-specific operations.
- Non-AI digital goods (eSIM, gift cards, inventory, orders, fulfilment, and payment orchestration) belong to the commerce platform. Integrate through explicit service contracts; do not embed commerce workflows in the gateway.
- Optimize for an open-source, fast, cost-conscious MVP. Reuse inherited New API capabilities before replacing them.
- Do not split services, replace the database, or introduce a second gateway/runtime without evidence and an accepted ADR.

## Domain language

Use these terms consistently:

- **Provider:** upstream vendor or integration family (for example, OpenAI or Anthropic).
- **Provider Account:** an upstream customer/tenant identity or credential set. This is a domain concept; it is not currently a generic first-class database entity.
- **Channel:** the current New API routable upstream connection. It carries configuration such as type, key, base URL, models, group, status, priority, weight, and balance.
- **Credential:** a static API key or a provider-plugin credential record.
- **Model:** a client-visible model identifier and its upstream mapping.
- **Capability:** a feature a provider/account/model can actually perform, such as vision, tool calling, structured output, audio, or reasoning.
- **Routing policy:** the constraints and selection rules used to choose an eligible Channel/account.

Do not use Provider, Account, Channel, Credential, and Model interchangeably.

## Current versus target

- **CURRENT** means verified in the checked-in source at the inspected commit.
- **TARGET** means a desired state, not necessarily implemented.
- **PROPOSED** means a recommendation awaiting an owner decision.
- **OPEN** means evidence or a decision is missing.
- Every new architecture document or diagram must label proposed entities and flows clearly.
- A roadmap item does not authorize implementation. Obtain an explicit task/decision and define acceptance criteria before coding.

## Change control

- Before adding a database entity or changing a domain boundary, inspect current models, migrations, cache behavior, controllers, services, and API consumers.
- For a proposed new entity, document identity, ownership, lifecycle, uniqueness, concurrency, deletion/retention, tenant scope, migration/backfill, compatibility, and rollback.
- Keep the existing Go module boundaries. `relaykit/` and `tokenkit/` must remain independently buildable.
- Preserve upstream project identity, license, attribution, and module-path protections in root `AGENTS.md`.
- Prefer an ADR for decisions with cross-module impact, data migration, public contract changes, provider integration strategy, routing semantics, security boundaries, or operational topology.

## Required task sequence

1. Inspect the current code and tests; capture the exact branch/commit and relevant paths.
2. Compare current behavior with the documented requirement.
3. Propose the smallest change that satisfies the requirement.
4. Implement only approved scope.
5. Run focused tests and every applicable mandatory verification matrix.
6. Update docs and ADR status when behavior or decisions change.
7. Report evidence, failures, and unresolved questions.

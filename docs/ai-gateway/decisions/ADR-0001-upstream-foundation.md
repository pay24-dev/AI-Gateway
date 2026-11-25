# ADR-0001: Use New API as the gateway foundation

- **Status:** Accepted
- **Date:** 2026-11-25
- **Decision owners:** Project owner
- **Scope:** Repository foundation

## Context

The project needs an open-source AI gateway that can reach a production-capable MVP quickly and at low implementation cost. Candidate projects included Bifrost and New API. The repository was created as a fork of `QuantumNous/new-api`; at initial inspection its `main` head matched the upstream `main` commit `c27abf36268001ebfcf4f478ba0f7bae1c19de22`.

The inherited code already includes provider channels, channel-level priority/weight, model mapping, usage/billing, admin UI, multi-key handling, and a provider-plugin extension surface.

## Decision

Use the existing New API codebase as the foundation. Do not replace it with Bifrost or build a second gateway for the MVP. Evaluate Bifrost concepts as design references only when they solve a concrete gap and can be adapted without duplicating core routing.

## Alternatives considered

- **Bifrost as the base:** strong developer-oriented gateway architecture, but would require rebuilding or integrating more of the account-management, admin, usage, and operational surface already present in this repository.
- **New API as the base:** selected because it provides more ready-to-use gateway and management behavior and a larger community, reducing time-to-MVP.
- **Build a custom gateway:** rejected for the MVP due to cost, delivery time, and unnecessary ownership of protocol, routing, billing, and admin features.

## Consequences

- Preserve upstream architecture and module boundaries.
- Prefer extension over replacement.
- Maintain a clear upstream sync strategy and test downstream patches after sync.
- Preserve New API and QuantumNous attribution, AGPL obligations, module paths, and required UI links.
- Avoid broad refactors that make upstream updates difficult.

## Follow-up

- Define how upstream commits are tracked and merged.
- Maintain a list of downstream patches and their rationale.
- Re-evaluate only if measured limitations block product requirements.

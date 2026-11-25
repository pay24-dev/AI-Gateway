# ADR-0004: Reuse the existing provider and request plugin surfaces

- **Status:** Proposed — owner confirmation required for new extension contracts
- **Date:** 2026-11-25
- **Scope:** Provider integrations and cross-cutting request behavior

## Context

The inherited repository already has native channel adapters, Advanced Custom routes, a JavaScript plugin runtime, a provider part, a request-processing part, and a task part. A new extension framework would duplicate substantial behavior.

## Proposed decision

Use the least complex existing extension surface:

1. Native channel adapter when a stable provider-specific adapter already exists or a native implementation is justified.
2. Compatible/Advanced Custom channel when the provider implements an existing protocol and configuration is sufficient.
3. Provider-plugin part for provider-specific credentials, login/refresh/revoke, signing, model discovery, request conversion, or balance.
4. Request-processing part for cross-cutting transformations/policies across channels.
5. Task part for asynchronous task APIs.

Do not build a second plugin runtime or provider registry for the MVP.

## Consequences

- Read the exact checked-in plugin API contract and schema before changing plugin behavior.
- The checked-in v1 docs identify the contract as unreleased; compatibility must be pinned and tested.
- Plugin source/version trust, publisher verification, signing, permissions, and revocation remain separate governance concerns.
- New native channel types require evidence that the existing extension points are insufficient.

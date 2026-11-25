# ADR-0002: Use separate Channels for independently routed accounts in the MVP

- **Status:** Proposed — owner confirmation required before implementation changes
- **Date:** 2026-11-25
- **Scope:** Provider account isolation and load balancing

## Context

The current `model.Channel` exposes channel-level credentials/configuration, models, status, balance, priority, and weight. The current selector performs weighted random selection among eligible Channels in the selected priority tier.

A multi-key Channel supports random and polling credential selection. It does not expose independent per-key weight or priority. Provider-plugin multi-key balance details may be stored per key index, but that does not make routing independently weighted per key.

## Proposed decision

For the MVP, represent each upstream account that needs independent routing behavior as its own Channel. Use separate Channels to obtain independent status, priority, weight, model mapping, base URL/settings, and channel-level diagnostics.

Use a multi-key Channel only when credentials are homogeneous and random/polling distribution is acceptable.

## Alternatives

- **One multi-key Channel for all accounts:** simpler configuration, but independent per-key policy is limited.
- **New ProviderAccount/Credential tables:** more normalized domain model, but requires migrations, compatibility, admin UI, API changes, cache behavior, and upstream sync cost.
- **Separate Channels:** reuses existing routing and management behavior; selected as the proposed MVP approach.

## Consequences

- Provider vendor identity is represented by the Channel type/adapter/plugin family.
- Account identity needs a stable naming/metadata convention that does not expose secrets.
- Channel-level balance must not be mistaken for an account's balance if a Channel contains multiple credentials.
- The admin UI may need clearer grouping of Channels by provider/account, but that is a later scoped feature.
- Per-key weighted routing remains a future requirement, not a current capability.

## Migration and rollback

No database migration is required for the proposed MVP mapping. If a future ProviderAccount entity is approved, define a backfill from existing Channels and plugin credentials, dual-read compatibility, rollback, and full database matrix first.

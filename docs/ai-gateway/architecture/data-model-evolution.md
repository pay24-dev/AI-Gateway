# Data-model evolution

**Status:** TARGET guidance; no schema change is authorized by this document.

## Current persistence approach

The current gateway uses GORM models and migrations in `model/`. The primary database supports SQLite, MySQL, and PostgreSQL; an optional separate log database also supports ClickHouse. Redis and in-memory caches are used for shared/runtime state.

The current `model.Channel` combines several concerns: provider/channel type, credentials, base URL, served models, model mapping, group, status, priority, weight, balance, and settings. Plugin multi-key state and per-key balance details extend this representation.

## Potential future entities

These are domain concepts only. Add a table only after a concrete requirement and accepted ADR.

| Candidate | Why it may be needed | Questions before implementation |
|---|---|---|
| Provider | Stable vendor metadata independent of Channel type | Is type/adapter metadata already sufficient? Who owns provider lifecycle? |
| ProviderAccount | First-class account identity and metadata | How does it map to existing Channel and plugin credentials? Can one account serve multiple Channels? |
| Credential | Independent secret lifecycle and policy | How are encryption, rotation, expiry, and migration handled? |
| ModelCapability | Capability by provider/account/model/protocol | What is the evidence source, expiry, override and unknown-state model? |
| RoutingPolicy | Reusable policy beyond Channel priority/weight | How does it compose with group, affinity, retries and existing selection? |
| ProviderHealth | Durable or cached health state | Which signals are authoritative and how is stale state handled? |

## Migration requirements

Any schema change must document:

1. Current schema and representative production data.
2. New identity/foreign-key and uniqueness rules.
3. Backfill mapping from Channel and existing credentials.
4. Dual-read/write or compatibility behavior during rollout.
5. Cache invalidation and multi-node consistency.
6. Rollback and data preservation.
7. Fresh database and upgrade-from-latest-release tests.
8. Real SQLite, MySQL, and PostgreSQL verification as required by root `AGENTS.md`.

## Database invariants

- Keep all three primary database dialects working.
- Do not add database-specific SQL without a verified cross-dialect path.
- Keep migrations idempotent.
- Avoid silently changing nullable/zero-value semantics.
- Preserve existing API payloads and model serialization.
- Use transactions/locking helpers according to existing `model/` conventions.
- Read `.agents/rules/billing.md` before any billing or quota data change.

## Recommendation

For the initial delivery, prefer the existing Channel representation and plugin credential model. Revisit normalized ProviderAccount and ModelCapability entities only when account lifecycle, capability provenance, or routing requirements cannot be represented safely with the current model.

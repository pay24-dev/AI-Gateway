# Product requirements

**Status:** Baseline requirements; implementation status is classified per row.

## Functional requirements

| ID | Requirement | Baseline status | Acceptance direction |
|---|---|---|---|
| FR-01 | Connect multiple authorized AI providers | CURRENT — native channel catalog and custom/plugin paths exist | A provider can be configured and tested without changing client integration |
| FR-02 | Manage multiple accounts for one provider | CURRENT — separate Channels and plugin multi-key credentials exist | Account identity and credential ownership are explicit and isolated |
| FR-03 | Independently route provider accounts | CURRENT — Channel-level priority and weight exist | Each independently governed account has independent status, models, weight, priority, and diagnostics |
| FR-04 | Distribute traffic by weight | CURRENT — weighted random selection applies to eligible Channels at a priority tier | Distribution is documented and testable; do not imply per-key weighting |
| FR-05 | Prioritize and fail over between Channels | CURRENT — priority tiers and retry selection exist | Failover follows classified error policy and request safety |
| FR-06 | Select credentials inside a multi-key Channel | CURRENT — random and polling modes | Per-key weighted selection is not claimed |
| FR-07 | Track balance by provider account/credential | CURRENT — channel balance and provider-plugin multi-key balance detail exist | Currency, unit, freshness, and partial-failure semantics are visible |
| FR-08 | Discover and map upstream models | CURRENT — channel model list/mapping and provider plugin model routes exist | Client-visible model identifiers map to valid upstream models |
| FR-09 | Record usage and calculate charges | CURRENT — inherited usage and billing subsystems | Changes preserve all billing invariants and database compatibility |
| FR-10 | Extend provider authentication and request conversion | CURRENT — provider-plugin hooks exist | New integrations use the existing extension path where it fits |
| FR-11 | Maintain a capability registry by account/model | TARGET | Routing can filter on verified capabilities, not assumptions based only on model names |
| FR-12 | Explain routing decisions | TARGET | A request trace identifies candidate filters, chosen Channel, and safe failure reasons |
| FR-13 | Apply advanced cost/latency/concurrency-aware routing | TARGET | Policies are measurable, bounded, deterministic where required, and do not bypass core routing |
| FR-14 | Integrate with commerce systems | TARGET — API boundary only | Commerce owns catalogue/order/payment/fulfilment; gateway owns AI execution |

## Non-functional requirements

| ID | Requirement | Direction |
|---|---|---|
| NFR-01 | Security | Secrets are encrypted/controlled, redacted from logs, and scoped to the owning account/channel |
| NFR-02 | Reliability | Retry only classified transient failures and only when request semantics permit it |
| NFR-03 | Compatibility | Preserve supported client protocols and document conversion limitations |
| NFR-04 | Maintainability | Reuse established adapters, services, plugin contracts, and database patterns |
| NFR-05 | Portability | Preserve supported SQLite, MySQL, and PostgreSQL behavior |
| NFR-06 | Observability | Expose safe request, provider, account, routing, and billing diagnostics |
| NFR-07 | Performance | Avoid synchronous external balance/model discovery calls on the request critical path |
| NFR-08 | Extensibility | Provider additions must have bounded contracts, tests, versioning, and rollback |
| NFR-09 | Cost | Prefer existing infrastructure and dependencies; justify new operational components |
| NFR-10 | Auditability | Sensitive configuration changes and account lifecycle actions are traceable without storing secrets in audit payloads |

## MVP acceptance baseline

Before declaring the MVP production-ready, demonstrate:

1. Two or more independently configured accounts for one provider.
2. Independent channel enable/disable, priority, and weight.
3. Model access and mapping per channel.
4. A successful request and a controlled retry/failover scenario.
5. Balance/usage visibility with clear units and freshness.
6. No credential leakage in API responses, logs, audit events, or error messages.
7. Database and regression tests required by root `AGENTS.md`.
8. Deployment, backup, secret rotation, and incident procedures documented.

The acceptance list is a target. It does not claim that all scenarios have been re-tested in this fork.

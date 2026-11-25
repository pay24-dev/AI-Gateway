# Production-readiness checklist

**Status:** Checklist; not a certification or claim that the current deployment is production-ready.

## Architecture and capacity

- [ ] Confirm deployment topology, expected concurrency, traffic profile, and failure domains.
- [ ] Load-test representative streaming and non-streaming requests with realistic provider latency.
- [ ] Define availability, latency, error-rate, and recovery targets from measurements.
- [ ] Confirm DB connection limits, Redis topology, cache consistency, and multi-node behavior.
- [ ] Confirm background balance/model discovery tasks do not overload providers or block request traffic.

## Provider accounts and routing

- [ ] Each independently routed upstream account has a clear Channel/account identity.
- [ ] Credential expiry, refresh, revoke, and disable behavior are tested.
- [ ] Priority, weight, model mapping, group and retry behavior are documented.
- [ ] Multi-key random/polling limitations are understood.
- [ ] Retry policy distinguishes permanent, transient, rate-limit, auth, quota, and client errors.
- [ ] Non-idempotent task submissions are not duplicated by unsafe retries.
- [ ] Balance units, currency, freshness, and unlimited/reset semantics are explicit.

## Security

- [ ] HTTPS and secure cookie/origin configuration are correct.
- [ ] Strong persistent session/crypto secrets are configured and shared consistently across nodes.
- [ ] Trusted proxies are explicitly configured for the real network.
- [ ] Provider credentials are stored, redacted, and rotated safely.
- [ ] Plugin source and versions are reviewed and pinned.
- [ ] Outbound URL/SSRF controls are verified.
- [ ] Authentication and authorization controls have focused regression tests.
- [ ] No secrets or sensitive payloads are present in logs, metrics, audit records, or backups without appropriate protection.

## Data and recovery

- [ ] Automated database backups and restore tests exist.
- [ ] Redis/cache loss behavior is understood.
- [ ] Migration and rollback procedures are rehearsed.
- [ ] Retention and deletion rules are defined for usage logs and audit data.
- [ ] SQLite/MySQL/PostgreSQL compatibility is verified for relevant changes.

## Operations

- [ ] Health checks and alerting cover gateway, database, Redis, provider failures, and task backlog.
- [ ] On-call owner and escalation path are defined.
- [ ] Provider outage and credential compromise runbooks exist.
- [ ] Logs have correlation IDs and safe error classification.
- [ ] Release, rollback, and configuration-change procedures are documented.
- [ ] Legal, tax, upstream authorization, and applicable regulatory obligations are reviewed for the deployment jurisdiction.

## Open decisions

- Numeric SLOs and error budgets.
- Production database topology and backup RPO/RTO.
- Plugin publisher trust and signing model.
- Capability verification source and refresh cadence.
- Per-account concurrency/rate-limit enforcement model.

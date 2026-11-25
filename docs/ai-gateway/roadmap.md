# Roadmap

**Status:** Proposed delivery sequence. The roadmap is not implementation authorization by itself.

## Phase 0 — repository and agent foundation

- [x] Confirm the repository is a fork of QuantumNous/new-api and inspect its baseline.
- [ ] Maintain a project-specific documentation index and current/target distinction.
- [ ] Establish Codex/Claude entry instructions and conditional engineering rules.
- [ ] Record accepted and proposed architecture decisions.
- [ ] Confirm legal attribution and AGPL obligations remain intact.

## Phase 1 — inherited baseline stabilization

- Map actual provider, channel, plugin, routing, billing, and model-discovery behavior.
- Establish repeatable local development and test procedures.
- Review upstream synchronization strategy and patch ownership.
- Identify security and operational gaps before adding product-specific code.
- Produce a verified provider/account inventory and capability gap list.

## Phase 2 — fast MVP using existing Channel model

- Configure one Channel per independently routed provider account.
- Use existing channel-level priority and weighted selection.
- Use existing model mapping, group, status, balance and health surfaces.
- Use provider plugins for vendor-specific login/refresh/signing/balance when needed.
- Add focused tests for account isolation, routing, retries, and credential redaction.
- Avoid a new ProviderAccount table unless an accepted ADR demonstrates necessity.

## Phase 3 — capability registry

- Define a canonical capability vocabulary and evidence model.
- Identify which capabilities can be inferred from provider/model metadata and which require probes or operator overrides.
- Store capability provenance, verification time, and unknown state.
- Integrate capability filtering into the existing routing pipeline only after an ADR and test plan.

## Phase 4 — advanced routing and operations

- Add explainable routing traces.
- Evaluate health/circuit-breaker signals, per-account concurrency and rate limits.
- Add cost/latency-aware routing only where measurements are reliable.
- Consider per-key weighted pools only if separate Channels are insufficient.
- Add operational dashboards and alerts for account-level behavior.

## Phase 5 — OxyChain platform integration

- Define authenticated service-to-service APIs and ownership boundaries.
- Keep commerce catalogue, checkout, payment, orders, inventory, and fulfilment in the commerce platform.
- Define AI product entitlement/provisioning and usage reconciliation contracts.
- Add integration tests without coupling gateway internals to commerce database models.

## Phase gates

A phase is complete only when:
- scope and acceptance criteria are agreed;
- behavior is implemented and reviewed;
- required tests and security checks pass;
- documentation matches the code;
- rollback and operational implications are documented;
- no unresolved blocker is hidden in a completion claim.

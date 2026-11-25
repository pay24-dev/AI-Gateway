# Product charter

**Status:** TARGET / owner direction  
**Last reviewed:** 2026-11-25

## Vision

Provide a self-hosted, provider-neutral AI gateway that lets an operator connect authorized upstream AI services, manage multiple accounts, expose stable client-facing API protocols, and route requests according to model support, account availability, priority, weight, and operational policy.

The gateway is an AI infrastructure component within the broader OxyChain product direction. It is not the general catalog, checkout, payment, inventory, or fulfilment system for every digital product.

## Product outcomes

1. **One client integration surface:** applications and agents use supported gateway API protocols instead of integrating each upstream independently.
2. **Provider choice:** operators can connect native providers, compatible endpoints, and supported plugins.
3. **Account isolation:** independent upstream accounts can be enabled, disabled, monitored, prioritized, and weighted without sharing accidental state.
4. **Reliable routing:** requests select only eligible channels/accounts and fail over only when the failure is safe and retryable.
5. **Transparent operations:** usage, estimated/settled cost, balance freshness, provider errors, latency, and routing decisions are observable.
6. **Extensibility:** new provider behavior and cross-cutting request policies can be added through the existing extension model where possible.
7. **Low-cost delivery:** reuse the existing New API codebase and infrastructure before adding new services or dependencies.

## Primary users

| User | Primary needs |
|---|---|
| Platform administrator | Configure providers, credentials, models, groups, policies, limits, and system settings |
| Provider operator | Inspect account health, balance, credential expiry, failures, and routing eligibility |
| Application developer | Use stable API keys and documented API protocols |
| Support/finance operator | Trace usage, pricing, quota, and settlement records without seeing secrets |
| Security/audit operator | Review access, configuration changes, and sensitive operations |

## In scope

- AI provider adapters and authorized provider integrations.
- Multiple upstream accounts/credentials per provider.
- Model discovery, mapping, aliases, and protocol compatibility.
- Channel/account selection, priority, weight, retries, and failover.
- Account/channel balance and health visibility.
- Usage tracking, pricing, quota, and billing behavior inherited from the gateway.
- Provider, request-processing, and asynchronous task plugin capabilities.
- Admin operations, auditability, deployment, and observability.

## Out of scope

- eSIM, gift cards, vouchers, inventory, catalogue, checkout, order state, payment orchestration, and fulfilment.
- General-purpose non-AI digital product providers.
- Acquiring or sharing upstream accounts without authorization.
- Circumventing provider rate limits, access controls, identity checks, or terms.
- Replacing all inherited gateway code before demonstrating a concrete limitation.
- A microservice rewrite as an initial delivery strategy.

Commerce workflows should remain in the commerce platform (Medusa is the planned commerce authority) and communicate with this gateway through explicit APIs if AI products need to be sold or provisioned.

## Product principles

- Reuse before replace.
- Explicit account boundaries.
- Provider-neutral domain concepts.
- API compatibility over provider-specific client lock-in.
- Secure-by-default credential handling.
- Observable routing decisions.
- Minimal schema changes and reversible migrations.
- No hidden coupling to commerce or payment systems.
- Documented decisions before cross-cutting architecture changes.

## Delivery constraints

- Source code remains open-source under the inherited AGPL-3.0 terms and notices.
- Prioritize a production-usable MVP quickly and at low implementation cost.
- Preserve required New API and QuantumNous identity/attribution as specified by repository policy and license.
- Keep the current Go monolith and React console until evidence justifies a different topology.

## Success criteria

The project is successful when an operator can add multiple authorized accounts for a provider, expose a stable client API, route across eligible accounts according to explicit policy, understand why an account was selected or skipped, inspect usage and balance safely, and add a provider without creating an unrelated parallel gateway subsystem.

Numeric SLOs, throughput targets, and cost targets are **OPEN** until measured against the intended deployment.

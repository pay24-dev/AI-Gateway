# Routing and balancing rules

Read this file before changing channel selection, provider-account distribution, priority, weight, retries, failover, multi-key selection, health, or routing observability.

## Verified current semantics

The current New API implementation already has:

- Channel-level `Weight` and `Priority` fields in `model.Channel`.
- Candidate filtering by group, model, and request constraints.
- Priority tiers ordered from higher priority to lower priority.
- Weighted random selection among eligible Channels at the selected priority tier in `model.GetRandomSatisfiedChannel` (`model/channel_cache.go`).
- Equal effective selection when all candidate Channel weights are zero.
- Retry logic in `service/channel_select.go` that advances through priority levels and supports configured group behavior.
- Multi-key credential selection in `model/channel.go` with `random` and `polling` modes only.
- Per-key status/disable metadata for multi-key channels.
- Provider-plugin multi-key balance details stored per key index in `model.ChannelBalanceDetail`.

Always re-check these files before changing semantics; this section is a map, not a substitute for source inspection.

## Important limitations

- Channel weight is not a per-key weight.
- Multi-key random/polling selection is not weighted distribution.
- A multi-key Channel shares channel-level routing configuration. Do not assume every credential line can have independent model mapping, priority, weight, base URL, or policy.
- Provider-plugin per-key balance details do not automatically provide per-key routing weights or a general capability registry.
- Do not infer that balance means remaining usable quota unless the provider's documented semantics establish that.

## MVP recommendation

For accounts that require independent routing behavior, represent each upstream account as its own Channel:

- independent credentials and, where required, base URL/config;
- independent model list/mapping;
- independent status, priority, and weight;
- independent channel-level balance/health visibility;
- the same provider type/vendor family across multiple Channels.

Use a multi-key Channel only when the credentials are sufficiently homogeneous and random or polling distribution is acceptable.

This is the MVP recommendation documented in ADR-0002. It is not permission to add a new ProviderAccount table.

## Future routing policy

A future account-aware router may evaluate, in order:

1. protocol and model eligibility;
2. capability requirements (vision, tools, structured output, audio, reasoning, etc.);
3. account/channel status, credential validity, balance freshness, and provider health;
4. group and priority tier;
5. weighted selection within the eligible tier;
6. retry/failover only for classified retryable failures;
7. cost, latency, concurrency, and regional preferences when their data is reliable.

Do not implement this as a parallel router. Extend the existing selection pipeline behind tested interfaces and an ADR.

## Correctness requirements

- Do not retry invalid client requests as if they were provider failures.
- Distinguish authentication, quota, rate-limit, transient server, network, timeout, and protocol/conversion failures.
- Avoid retrying non-idempotent requests unless the upstream contract and request lifecycle make it safe.
- Keep request affinity and group semantics intact.
- Make routing decisions diagnosable without logging secrets or full sensitive prompts.
- Test weighted distribution deterministically where possible; avoid timing-based or flaky statistical tests.

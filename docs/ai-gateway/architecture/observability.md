# Observability and operational signals

**Status:** CURRENT inherited logs/performance surfaces; additional account-aware diagnostics are TARGET.

## Existing signals to inspect

The current source includes channel test/response-time/balance fields, usage and relay logs, performance-related models/controllers, system tasks, and provider-plugin balance details. Exact dashboard coverage must be verified in the current frontend and API before claiming it is exposed.

Relevant starting points include:

- `model/channel.go`
- `model/channel_balance.go`
- `model/perf_metric.go`
- `controller/performance.go`
- `service/channel_select.go`
- `service/provider_plugin_balance.go`
- `service/log_info_generate.go`

## Target signals

| Area | Useful signals |
|---|---|
| Request | Request count, latency, streaming duration, status/error class |
| Provider/account | Attempts, success rate, rate limits, auth failures, timeout rate |
| Routing | Candidate count, filters applied, selected Channel, priority tier, retry/failover count |
| Balance | Amount, currency, unlimited flag, reset windows, last successful check, error |
| Capability | Evidence source, verification time, unknown/unsupported state |
| Usage/billing | Input/output/cache tokens, modality-specific quantities, estimated and settled charge |
| Plugin | Plugin version, hook failures, execution duration, disabled state |
| Infrastructure | CPU, memory, DB latency, cache errors, queue/task backlog |

These are proposed observability requirements, not a claim that every metric currently exists.

## Logging principles

- Correlate each request across client, relay, upstream attempt, and settlement using safe request identifiers.
- Record routing decisions with stable reason codes.
- Never include secrets or full sensitive prompts in metrics.
- Keep high-cardinality labels (user IDs, raw model payloads, tokens) out of metrics.
- Distinguish client errors, provider errors, plugin errors, and internal errors.
- Record balance freshness; an old successful balance is not equivalent to a current balance.

## Operational targets

Availability SLOs, latency percentiles, retention periods, alert thresholds, and error budgets are OPEN until baseline traffic and deployment topology are measured. Do not invent numeric SLOs in code or dashboards.

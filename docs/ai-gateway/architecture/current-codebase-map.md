# Current codebase map

**Status:** CURRENT at the inspected baseline. Re-check paths after upstream synchronization.

## Backend request and business layers

| Path | Responsibility |
|---|---|
| `main.go` | Application entry point |
| `router/` | HTTP route registration |
| `middleware/` | Authentication, access, request and response middleware |
| `controller/` | HTTP handlers and API orchestration |
| `service/` | Business logic, routing orchestration, provider plugin lifecycle, usage and operations |
| `model/` | GORM models, persistence, caches, channel state and migrations |
| `dto/` | Request/response and relay data transfer objects |
| `constant/` | Channel types, modes, endpoint and context constants |
| `common/` | Shared utilities, serialization, errors and configuration |
| `setting/` | Runtime settings and feature configuration |
| `logger/` | Logging and request diagnostics |

## Relay and provider layers

| Path | Responsibility |
|---|---|
| `relay/` | Protocol handlers, channel adapters, upstream relay, request billing integration |
| `relay/channel/` | Native provider channel adapters |
| `relay/channel/providerplugin/` | Provider-plugin relay adaptor, conversion chain, stream handling |
| `relaykit/` | Independent Go module for protocol DTOs and conversions |
| `tokenkit/` | Independent Go module for token and image estimation |
| `pkg/jsplugin/` | JavaScript plugin engine, provider contracts, registry, routing and hooks |
| `plugins/` | Built-in plugin implementations and tests; do not add docs here |

## Provider-plugin lifecycle paths

- `model/channel.go`: Channel and ChannelInfo, including multi-key metadata.
- `model/channel_balance.go`: persisted channel and per-key balance detail.
- `service/provider_plugin.go`: provider-plugin channel integration.
- `service/provider_plugin_login.go`: login and credential lifecycle.
- `service/provider_plugin_balance.go`: balance collection and aggregation.
- `controller/provider_plugin.go`: admin endpoints for plugin login and credential summaries.
- `docs/plugin-api/v1.md`: detailed checked-in plugin contract.
- `docs/plugin-api/v1.d.ts` and `v1.schema.json`: typed and machine-readable contract surfaces.

## Frontend and packaging

| Path | Responsibility |
|---|---|
| `web/` | React admin console; read `web/AGENTS.md` before frontend changes |
| `web/src/features/` | Feature-oriented UI modules |
| `web/src/routes/` | TanStack Router route tree |
| `web/src/i18n/` | Frontend localization |
| `electron/` | Desktop wrapper |
| `Dockerfile*`, `docker-compose*.yml` | Container build and deployment |
| `.github/workflows/` | CI, image build and release automation |

## Routing-specific paths

- `model/channel_cache.go`: candidate selection, priority tier ordering, and weighted Channel selection.
- `model/channel.go`: multi-key credential selection and Channel accessors.
- `service/channel_select.go`: request-level group, retry, and channel-selection orchestration.
- `model/channel_priority.go`: priority reorder model and persistence behavior.

## Existing documentation and policies

- `AGENTS.md`, `CLAUDE.md`, `web/AGENTS.md`: agent and contributor instructions.
- `.agents/rules/billing.md`: mandatory billing invariants.
- `.agents/github/`: issue and PR evidence rules.
- `docs/authentication.md`: inherited authentication guide.
- `docs/plugin-api/`: plugin API contract.
- `docs/openapi/`: API specifications.
- `docs/ai-gateway/`: project-specific product and architecture overlay.

## Module-path warning

The root Go module and nested modules currently use `github.com/QuantumNous/new-api` paths. These paths are protected by repository policy and are not to be renamed as part of ordinary project setup. A repository URL/name change does not automatically imply a Go module-path migration.

# AI Gateway — project documentation

This directory is the project-specific product and architecture overlay for the `pay24-dev/AI-Gateway` repository. The checked-in application is a downstream fork of [QuantumNous/new-api](https://github.com/QuantumNous/new-api); inherited source identity, attribution, license obligations, module paths, and required UI links remain protected by the root `AGENTS.md` and repository license.

This documentation does **not** replace the inherited upstream README, installation guides, authentication guide, OpenAPI files, or plugin API contract.

## Project snapshot

| Item | Current evidence |
|---|---|
| Repository | `pay24-dev/AI-Gateway` |
| Upstream parent | `QuantumNous/new-api` |
| Initial inspected baseline | `c27abf36268001ebfcf4f478ba0f7bae1c19de22` |
| Initial baseline relationship | Fork `main` matched upstream `main` at inspection time |
| License | AGPL-3.0, with inherited additional attribution terms in the root README |
| Backend | Go, Gin, GORM |
| Frontend | React 19, TypeScript, Rsbuild, Bun |
| Data/cache | SQLite, MySQL, PostgreSQL; optional ClickHouse log DB; Redis/in-memory cache |
| Extension runtime | JavaScript plugin runtime; provider, request-processing, and task plugin surfaces |

The snapshot above describes the inspected baseline, not a guarantee that the branch has not changed since.

## Read this first

1. [Product charter](product/charter.md)
2. [Requirements and status](product/requirements.md)
3. [System overview](architecture/system-overview.md)
4. [Current codebase map](architecture/current-codebase-map.md)
5. [Domain model](architecture/domain-model.md)
6. [Routing and balancing](architecture/routing-and-balancing.md)
7. [Provider integrations](architecture/provider-integrations.md)
8. [Agent workflow](development/agent-workflow.md)
9. [Roadmap](roadmap.md)

## Documentation map

### Product

- [Charter, scope, users, and constraints](product/charter.md)
- [Functional and non-functional requirements](product/requirements.md)

### Architecture

- [System overview and boundaries](architecture/system-overview.md)
- [Current repository/code map](architecture/current-codebase-map.md)
- [Domain model: current versus target](architecture/domain-model.md)
- [Provider integration strategy](architecture/provider-integrations.md)
- [Routing, priority, weight, multi-key, and failover](architecture/routing-and-balancing.md)
- [Plugin surfaces and extension rules](architecture/plugin-system.md)
- [API protocols and compatibility](architecture/api-and-compatibility.md)
- [Security and credential handling](architecture/security-and-secrets.md)
- [Data-model evolution and migration guardrails](architecture/data-model-evolution.md)
- [Observability and operational signals](architecture/observability.md)

### Engineering and operations

- [Agent operating workflow](development/agent-workflow.md)
- [Local development and verification](development/local-development-and-testing.md)
- [Production-readiness checklist](operations/production-readiness.md)
- [Roadmap and delivery phases](roadmap.md)

### Architecture Decision Records

- [ADR-0001: New API as the gateway foundation](decisions/ADR-0001-upstream-foundation.md)
- [ADR-0002: Account isolation using Channels for the MVP](decisions/ADR-0002-account-channel-routing.md)
- [ADR-0003: AI gateway versus commerce boundary](decisions/ADR-0003-commerce-boundary.md)
- [ADR-0004: Reuse existing extension surfaces](decisions/ADR-0004-extension-model.md)
- [ADR-0005: Open-source and license policy](decisions/ADR-0005-open-source-license.md)

## Status vocabulary

- **CURRENT** — verified in the inspected source.
- **TARGET** — desired state, not necessarily implemented.
- **PROPOSED** — recommendation that still needs explicit owner approval.
- **OPEN** — not yet verified or decided.
- **DEPRECATED** — retained for compatibility/migration only.

An agent must not convert a TARGET or PROPOSED statement into an implementation claim. Update the relevant document when code changes make a statement stale.

## Important existing capabilities

The inherited code already includes channel-level priority/weight routing, model mapping, groups, multi-key random/polling selection, provider-plugin credentials/login/refresh/balance hooks, usage accounting, and a web console. See the linked architecture pages for exact code paths and limitations.

In particular, per-key weighted balancing is **not** implied by channel-level weight or by multi-key random/polling mode. The MVP recommendation is to use separate Channels when provider accounts need independent routing policy.

## Agent entry points

- Codex and other repository agents: root `AGENTS.md` is mandatory.
- Claude Code: follow `CLAUDE.md` and explicitly read the complete root `AGENTS.md` at the start of every task.
- Conditional rules: `.agents/rules/`.
- Existing plugin contract: `docs/plugin-api/`.
- Frontend rules: `web/AGENTS.md`.

## Documentation maintenance

When a feature changes behavior, update the smallest authoritative page and relevant ADR/roadmap status. Keep current implementation facts separate from future design. Do not create documentation under `plugins/`.

# Agent instructions index

This repository is designed to be worked on by Codex, Claude Code, and other repository-aware agents.

## Read order

1. **Every agent, every task:** read the complete root [AGENTS.md](../AGENTS.md).
2. **Claude Code:** read [CLAUDE.md](../CLAUDE.md), then explicitly read the complete root `AGENTS.md` with the Read tool before any planning or project answer.
3. Read this index and the [AI Gateway documentation index](../docs/ai-gateway/README.md).
4. Read the conditional rule file that matches the task.
5. Read the relevant implementation and tests before planning changes.

## Sources of truth

| Source | Authority |
|---|---|
| Root `AGENTS.md` | Mandatory repository policy and upstream protections |
| `CLAUDE.md` | Claude Code session/read requirements |
| `.agents/rules/` | Conditional, task-specific engineering rules |
| `docs/ai-gateway/` | Product intent, current architecture, target design, ADRs, roadmap |
| `docs/plugin-api/` | Checked-in plugin API contract and schema |
| `web/AGENTS.md` | Frontend engineering conventions |
| `.agents/github/` | Issue and PR evidence/governance workflow |
| `.agents/skills/` | Repository-local agent skills |

If two documents conflict, follow system/developer instructions first, then root `AGENTS.md`, then the most specific applicable rule. Report unresolved contradictions instead of silently choosing.

## Conditional rules

- Product/domain/system architecture: `.agents/rules/ai-gateway-architecture.md`
- Provider adapters, provider plugins, credentials, OAuth, model discovery, balance: `.agents/rules/provider-integration.md`
- Channel selection, priorities, weights, multi-key, retries and failover: `.agents/rules/routing-and-balancing.md`
- Documentation, diagrams, ADRs and roadmap: `.agents/rules/documentation.md`
- Billing and quota: `.agents/rules/billing.md` (existing mandatory read gate in root `AGENTS.md`)

## Agent-neutral workflow

Use the same engineering standards regardless of agent:

`INSPECT → DIAGNOSE → PLAN → MINIMAL CHANGE → VALIDATE → REPORT`

Do not assume that a feature exists because it appears in a target design. Do not create parallel abstractions before inspecting inherited functionality. Do not claim checks passed unless they were run and their results observed.

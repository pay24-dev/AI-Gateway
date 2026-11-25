# Documentation and decision-record rules

Read this file before adding, changing, or reorganizing project documentation, diagrams, ADRs, agent instructions, or roadmap items.

## Documentation boundaries

- Project-specific product and architecture documents belong under `docs/ai-gateway/`.
- Inherited upstream operational, installation, authentication, API, and plugin documentation remains in its existing locations unless a user explicitly requests a change.
- Do not replace, hide, or remove protected upstream identity, author attribution, license terms, or legal links.
- Do not add documentation files under `plugins/` or plugin subdirectories.
- Keep agent rules in `.agents/rules/`; keep tool-specific entry instructions in `AGENTS.md` and `CLAUDE.md`.

## Evidence labels

Use these labels where status matters:

- **CURRENT** — directly verified in the checked-in implementation.
- **TARGET** — desired architecture, not a claim about current implementation.
- **PROPOSED** — recommendation awaiting an owner decision.
- **OPEN** — unknown or requiring research/decision.
- **DEPRECATED** — supported only for migration/compatibility and scheduled for removal.

Cite exact repository paths and, where useful, function/type names. Record the commit inspected when a document depends on implementation details.

## Writing standards

- Canonical technical and agent documentation is English to keep one shared working language across Codex, Claude, contributors, and code comments. User-facing product UI must continue to follow the repository i18n rules.
- Prefer one authoritative document per topic and link to it instead of copying large sections.
- Distinguish verified behavior, assumptions, target designs, and unresolved questions.
- Include context, constraints, examples, failure modes, security implications, migration/rollback, and acceptance criteria where relevant.
- Keep diagrams consistent with the code and label future entities/flows as proposed.
- Do not invent API endpoints, database columns, provider behavior, benchmark numbers, SLOs, or compatibility claims.

## ADR rules

Create or update an ADR when a decision changes a domain boundary, public contract, persistence model, provider integration strategy, routing semantics, security boundary, or operational topology.

Each ADR must include: status, date, context, decision, alternatives, consequences, migration/rollback implications, and follow-up work. Do not mark a proposed decision accepted without explicit owner approval.

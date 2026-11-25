# CLAUDE.md — Project Conventions for new-api

## MANDATORY: Read AGENTS.md with the Read tool

Do not treat `@AGENTS.md` as loaded. Claude Code does not reliably inline that import.

Before any planning, coding, reviewing, or answering a project question, you MUST call the Read tool on the repo-root file `AGENTS.md` and wait for the full contents. This is the first action of every session and every new task.

Rules:

- Do not start from memory, summaries, or this file alone.
- Do not skip the Read because a previous turn mentioned AGENTS.md.
- Do not replace the Read with a grep, glob, or partial skim.
- After reading, follow every rule in `AGENTS.md` for the rest of the work.
- If the task touches `web/`, also Read `web/AGENTS.md` before editing frontend files.
- If the task touches billing as defined under **Billing rules (mandatory read gate)** in `AGENTS.md`, also Read `.agents/rules/billing.md` in full before planning or editing. Tasks outside that definition may skip it.

---

## AI Gateway workspace instructions

This repository is a downstream workstream based on the protected New API codebase. Preserve all upstream project identity, author attribution, license notices, module paths, and UI-link requirements. Do not rewrite inherited README/legal material to hide or replace upstream identity.

After the mandatory root `AGENTS.md` read:

1. Read `.agents/README.md` and `docs/ai-gateway/README.md`.
2. Read the relevant conditional rule under `.agents/rules/` before planning.
3. For provider-plugin work, inspect `docs/plugin-api/v1.md`, `docs/plugin-api/v1.d.ts`, and `docs/plugin-api/v1.schema.json`; do not rely on a summary.
4. Separate current implementation from target architecture. A diagram or roadmap item is not permission to implement a new entity or subsystem.
5. Inspect the actual code path and tests before proposing or changing implementation. Preserve the existing New API module boundaries and compatibility contracts.
6. Finish with exact validation evidence and unresolved items. Do not report unrun checks as passed.

Codex should follow the root `AGENTS.md` and conditional rule files. Claude Code must explicitly read the full root `AGENTS.md` at the start of every new task, as required above.


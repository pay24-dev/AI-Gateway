# Agent operating workflow

**Audience:** Codex, Claude Code, and human contributors.

## Mandatory context

- Read root `AGENTS.md` in full for every new task.
- Claude Code must follow `CLAUDE.md` and explicitly use its Read tool for the complete root file.
- Read `.agents/README.md` and this documentation index.
- Read conditional rule files for the touched domain.
- For frontend work, read `web/AGENTS.md` and the applicable local skill.
- For billing work, read `.agents/rules/billing.md` in full.
- For plugin work, read the exact plugin API docs, type definitions, schema, and runtime validation.

## Task execution

### 1. Inspect

- Read `git status --short`, current branch, and current commit.
- Inspect the actual implementation, callers, persistence, caches, tests, and relevant docs.
- Search for existing functionality before creating a new abstraction.
- Identify protected files, module boundaries, and any mandatory test matrix.

### 2. Diagnose

- State the observed behavior and the expected behavior.
- Separate verified facts from hypotheses.
- Trace failure paths and side effects.
- Identify API, database, cache, plugin, billing, and security implications.

### 3. Plan

For non-trivial tasks, create a bounded plan with:
- files/modules likely to change;
- behavior and contracts to preserve;
- tests and commands to run;
- migration/rollback requirements;
- unresolved decisions.

Do not implement speculative target architecture just because it appears in a diagram.

### 4. Change

- Make the smallest coherent change.
- Preserve existing public APIs and upstream identity unless explicitly authorized and allowed by repository policy.
- Avoid unrelated refactors, dependency churn, broad formatting, or generated-file noise.
- Update tests and the authoritative documentation page together.
- Keep commits focused.

### 5. Validate

Run the narrowest meaningful checks first, then all mandatory checks for the affected layer. Capture actual output and exit status. Never state that tests passed based on code inspection.

### 6. Report

Every completion report should state:
- outcome and files changed;
- behavior changed or documentation added;
- commands/tests actually run and observed result;
- failures or checks not run;
- remaining assumptions, risks, or follow-up decisions.

## Agent collaboration

- Do not let two agents edit the same files concurrently without explicit ownership.
- Assign bounded tasks with exact files, acceptance criteria, and a clear no-touch list.
- Have one integrator reconcile changes, run the combined tests, and update documentation.
- Treat generated output and agent summaries as untrusted until checked against the working tree.
- Do not ask an agent to "implement the whole roadmap" in one task.

## Suggested task prompt shape

```text
Objective:
Current observed behavior:
Expected behavior:
Scope:
Out of scope / no-touch:
Relevant docs and rules:
Acceptance criteria:
Required tests:
Report format:
```

## Review checklist

- Does the change solve the requested problem without duplicating existing behavior?
- Are data ownership and account boundaries correct?
- Are retries safe and errors classified?
- Are secrets and sensitive usage data protected?
- Are database and module boundaries preserved?
- Are tests meaningful and deterministic?
- Are docs and ADR status consistent with code?

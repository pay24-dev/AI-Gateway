# Security and credential handling

**Status:** Security requirements for current and future work.

## Assets and trust boundaries

| Asset | Main risk | Required control |
|---|---|---|
| Provider API keys and OAuth tokens | Account takeover, unauthorized upstream use | Secret storage, scope isolation, redaction, rotation |
| User/API access tokens | Unauthorized gateway access | Server-side authorization, expiry, revocation, audit |
| Session and signing secrets | Session forgery or cross-node inconsistency | Strong random values, secure deployment, controlled rotation |
| Provider/plugin code | Credential exfiltration or request tampering | Source review, version pinning, runtime limits, restricted egress |
| Usage and billing records | Financial integrity and privacy | Validation, immutable audit evidence, safe arithmetic |
| Request/response payloads | Sensitive user data leakage | Minimize logging, redact, access-control logs |

## Credential handling rules

- Never log raw API keys, OAuth access/refresh tokens, authorization headers, callback codes, private keys, or usable session tokens.
- Do not return plugin credential data, headers, or query secrets in admin summaries. Return only the intended non-secret account metadata.
- Scope credential use to the Channel/account that owns it.
- Encrypt secrets at rest where the existing model supports it; never invent a new encryption scheme. Inspect current key storage and `CRYPTO_SECRET`/session handling before changing it.
- Keep secrets out of frontend bundles, local storage, analytics, issue reports, PR descriptions, and test snapshots.
- Rotate credentials and application secrets through documented operational procedures.

## Provider and plugin network safety

- Validate provider URLs and redirects against the host's existing protected outbound-fetch rules.
- Prevent SSRF to loopback, link-local, private, metadata, and internal control-plane addresses unless a specific administrator-controlled deployment requirement safely allows it.
- Use explicit timeouts, response-size limits, and safe redirect behavior.
- Do not disable TLS verification to make a provider work.
- Do not accept provider-supplied callback URLs or arbitrary destinations without validation.
- Treat plugin code as privileged executable code; restrict and review what it can access.

## Authentication and authorization

Root `AGENTS.md` requires applicable OWASP ASVS and Cheat Sheet guidance for authentication-related changes. Follow that mandatory read gate and test expiry, replay, session rotation/invalidation, CSRF/origin checks, privilege escalation, and alternate authentication paths as relevant.

All sensitive admin actions must be authorized server-side. UI hiding is not an authorization control.

## Logging and audit

- Log request IDs, provider/channel identifiers, safe error classes, timings, and routing outcomes.
- Redact credentials and sensitive payloads before logging.
- Audit credential add/refresh/revoke and sensitive channel changes without recording secret material.
- Avoid placing prompts or full provider responses in high-cardinality metrics or routine logs.
- Keep access to detailed usage and audit data role-controlled.

## Incident response

Document credential revocation, provider account disablement, affected-user identification, token rotation, log preservation, and safe recovery before public production operation.

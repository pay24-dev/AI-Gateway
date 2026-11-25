# Plugin and extension architecture

**Status:** CURRENT extension surfaces; future marketplace governance is OPEN.

## Do not conflate plugin types

The current repository has distinct plugin surfaces. Their responsibilities are different.

| Surface | Responsibility | Typical use |
|---|---|---|
| Provider part of Plugin API | Defines a provider channel, credentials, auth lifecycle, relay routes, conversion and balance | New upstream vendor with custom auth or protocol behavior |
| Request-processing part | Hooks into requests/responses across channels and scopes | Redaction, policy checks, request transformation, safe reporting |
| Task plugin part | Implements asynchronous task APIs and polling/rendering | Image/video/other asynchronous provider tasks |

A plugin may expose more than one part, but each contract and lifecycle must be understood separately.

## Provider part

The provider part can define relay routes, channel config, credentials, and hooks such as login, refresh, revoke, signing, key checks, scheduled maintenance, request conversion, and balance operations. The host retains responsibilities such as channel selection, model mapping, transport, persistence, retries, usage/billing, and settlement according to the checked-in contract.

Read the complete provider section of `docs/plugin-api/v1.md` before authoring or reviewing a provider plugin. Also inspect `v1.d.ts`, `v1.schema.json`, runtime validation, and tests.

## Request-processing part

The request-processing part supports stages such as ingress, request, outbound, egress, reply, and report. Instances may be scoped globally, by group, model, or Channel according to the contract. The host owns request lifecycle and applies hooks in a defined order.

Use this surface for cross-cutting request behavior. Do not add a provider adapter merely to redact or inspect a request.

## Task part

The task part handles asynchronous submission, polling, result rendering, and task-specific usage facts. The host retains persistence, retries, polling lifecycle, billing, and settlement boundaries described in the plugin contract.

Use it for asynchronous task APIs, not as a generic substitute for synchronous model relay.

## Plugin safety and lifecycle

- Treat plugins as executable code with access to provider credentials and request data within their contract.
- Pin and review plugin source/version; do not install untrusted code in production.
- Validate manifests and schemas before activation.
- Keep plugin secrets out of logs and browser-visible configuration.
- Bound memory, request sizes, execution time, outbound hosts, and response sizes.
- Define plugin disable/update/rollback behavior.
- Preserve compatibility with in-flight requests and persisted tasks when updating a plugin.
- Add tests for invalid manifests, missing hooks, runtime failures, timeout, malformed outputs, and security boundaries.

## Marketplace

A repository or marketplace of plugins is not, by itself, a security review or publisher verification system. Publisher identity, provenance, signing, version pinning, compatibility, permissions, and revocation are separate governance requirements. Their target design is OPEN.

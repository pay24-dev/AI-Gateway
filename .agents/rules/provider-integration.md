# Provider integration rules

Read this file before changing native provider adapters, provider-plugin channels, credentials, OAuth/login, refresh/revoke, model discovery, signing, balance checks, or provider-specific request conversion.

## Discovery before implementation

1. Search the current native channel/adaptor catalog and existing compatible-provider paths.
2. Check whether the upstream supports an existing protocol (OpenAI-compatible, Anthropic Messages, Gemini, or another implemented format).
3. Inspect the exact request, response, streaming, error, authentication, model-list, and balance behavior from official provider documentation.
4. Reuse an existing adapter or Advanced Custom channel when it correctly represents the provider contract.
5. Use the provider-plugin API when provider-specific authentication, credential lifecycle, signing, request conversion, model discovery, or balance logic requires it.
6. Add a native channel type only when the plugin/custom route cannot meet a documented requirement and the long-term maintenance cost is justified.

## Existing extension surface

The checked-in source includes a provider part in the plugin API and a provider-plugin runtime. The channel type constant is `ChannelTypeTaskPlugin = 61`. Provider-plugin manifests can define relay routes, channel configuration, credentials, and hooks. Relevant implementation/documentation includes:

- `docs/plugin-api/README.md`
- `docs/plugin-api/v1.md`
- `docs/plugin-api/v1.d.ts`
- `docs/plugin-api/v1.schema.json`
- `pkg/jsplugin/provider.go`
- `pkg/jsplugin/routing.go`
- `relay/channel/providerplugin/`
- `service/provider_plugin.go`
- `service/provider_plugin_login.go`
- `service/provider_plugin_balance.go`
- `controller/provider_plugin.go`

The checked-in plugin documentation identifies the v1 contract as unreleased. Verify the contract against the exact commit and tests; do not assume a published stable compatibility promise.

## Credential and account safety

- Never log, expose, or place raw credentials, refresh tokens, OAuth codes, private keys, or usable session tokens in issue/PR text, audit details, telemetry, browser-visible summaries, or error messages.
- Preserve redaction behavior. A credential summary may expose non-secret identity/expiry/status metadata, but not credential data, headers, or query secrets.
- Use the host-managed login/refresh/revoke/signing hooks where supported. Do not duplicate token lifecycle logic in unrelated controllers.
- Validate redirect state, callback origin, issuer, token audience, expiry, and one-time/replay behavior according to the provider protocol and existing security model.
- Do not trust provider-supplied URLs. Follow the host's protected outbound-fetch and URL validation paths; avoid SSRF, DNS rebinding, redirect, and private-network access.
- Set explicit request timeouts, response-size limits, and safe error handling. Do not weaken TLS verification as a provider workaround.
- Keep credentials scoped to the Channel/account that owns them. Do not accidentally reuse one account's token, balance, or model permissions for another.

## Provider integration checklist

- [ ] Official API and authentication contract identified.
- [ ] Existing channel/adaptor/plugin path checked.
- [ ] Request and response conversion covered.
- [ ] Streaming behavior covered where supported.
- [ ] Error classification and retry behavior checked.
- [ ] Model discovery and mapping checked.
- [ ] Credential lifecycle and redaction checked.
- [ ] Balance semantics and currency/units checked, if applicable.
- [ ] Account isolation and multi-key behavior checked.
- [ ] Tests cover success, invalid credentials, expiry/refresh, rate limits, transient failures, and malformed responses as applicable.
- [ ] Documentation describes limitations and upstream-specific behavior without claiming unsupported parity.

# Provider integration architecture

**Status:** CURRENT extension paths and TARGET integration workflow.

## Integration choices

Use the least complex path that faithfully represents the provider contract.

| Path | Use when | Maintenance profile |
|---|---|---|
| Existing native channel | The provider is already supported | Preferred; reuse existing tests and relay behavior |
| OpenAI/Anthropic/Gemini-compatible or Advanced Custom channel | The upstream implements a supported protocol and only configuration/routes differ | Low custom-code cost; verify edge cases |
| Provider Plugin | Authentication, credential lifecycle, signing, request conversion, model discovery, or balance needs provider-specific logic | Extensible without a new native channel type, but contract/runtime must be tested |
| New native channel type | Existing native/custom/plugin paths cannot represent a stable, important contract | Highest ongoing maintenance; require ADR and owner approval |

## Existing provider-plugin capabilities

The checked-in plugin API includes a provider part that can define:

- `meta.relay` routes and supported protocol conversions;
- per-channel `meta.channelConfig`;
- credentials and account identity metadata;
- login, refresh, revoke, and request-signing hooks;
- key checks and scheduled maintenance hooks;
- request/response/stream conversion hooks;
- balance request and parse hooks, including chained requests.

The provider-plugin channel type is `ChannelTypeTaskPlugin = 61` in the inspected source. The API is documented in `docs/plugin-api/v1.md`; the v1 docs identify the contract as unreleased, so agents must verify exact compatibility against the checked-in code and schema.

## Provider onboarding workflow

1. **Identify the vendor contract:** official API docs, base URL, auth, supported models, request formats, streaming, errors, limits, usage and balance semantics.
2. **Check existing support:** search native channel types, relay adapters, custom channel routes, built-in plugins, and tests.
3. **Choose integration path:** record why the selected path is sufficient and why simpler alternatives are not.
4. **Define model behavior:** upstream model IDs, aliases, model list discovery, client-facing names, capability claims and unsupported features.
5. **Implement authentication:** static key or plugin-managed credential lifecycle; handle refresh/revoke/expiry safely.
6. **Implement relay behavior:** request conversion, response conversion, stream events, error mapping, timeout and retry semantics.
7. **Implement balance only when meaningful:** document currency, unit, subscription/unlimited state, reset windows, partial results, and freshness.
8. **Test:** success, invalid credentials, expired credentials, refresh, rate limits, provider errors, malformed responses, stream interruption, and account isolation as applicable.
9. **Document and release:** provider setup, model limitations, config fields, plugin/API version, known gaps, and rollback.

## Account isolation

A provider vendor may have multiple independent upstream accounts. Account-specific configuration must not leak across accounts:

- credential and refresh state;
- model availability and mapping;
- account-specific base URL/region;
- balance and quota;
- health and disable status;
- routing weight and priority.

For the MVP, use separate Channels when these properties need independent routing. Use multi-key provider-plugin credentials only when the shared Channel semantics are acceptable.

## Evidence requirements

Do not infer provider support from a marketing model list alone. Preserve the source of capability evidence and identify whether it is provider-documented, verified by a live probe, or operator-configured. Never advertise unsupported protocol parity.

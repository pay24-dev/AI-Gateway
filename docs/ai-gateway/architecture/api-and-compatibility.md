# API protocols and compatibility

**Status:** CURRENT supported surfaces are inherited; exact behavior is endpoint/provider-specific.

## Client-facing protocols

The inherited README and OpenAPI files describe supported API families including:

- OpenAI Chat Completions and Responses;
- Anthropic Messages;
- Gemini generate-content endpoints;
- embeddings and reranking;
- image and audio endpoints;
- selected task/video and WebSocket paths where the provider and conversion path support them.

The presence of an endpoint does not mean every provider supports every field or mode. Compatibility depends on the upstream provider, channel type, converter, plugin, and request mode.

## Compatibility rules

1. Preserve existing request and response contracts unless a task explicitly changes them.
2. Do not claim full protocol parity from a successful basic text request.
3. Test non-stream and stream paths separately.
4. Test tool calls, reasoning fields, structured output, multimodal payloads, usage reporting, and error mapping where applicable.
5. Preserve unknown provider extensions only where the contract permits it; do not silently drop billing or security-relevant fields.
6. Keep API key authentication and group/model authorization server-side.
7. Document known conversion losses and provider-specific limitations.

## API source of truth

- `docs/openapi/api.json`: management/application API specification.
- `docs/openapi/relay.json`: relay API specification.
- `docs/plugin-api/`: plugin contracts.
- `router/`, `controller/`, DTOs and relay handlers: executable behavior.

When the specification and code disagree, identify the discrepancy and do not silently treat either as correct. Update the appropriate contract as part of an explicitly scoped task.

## Commerce integration boundary

The AI Gateway exposes AI execution APIs. Commerce systems may create entitlements or provision gateway access through explicit authenticated service APIs, but the gateway does not own catalogue, pricing of non-AI goods, checkout, payment, stock, order lifecycle, or fulfilment.

---
name: summarize-url
description: Summarize an article or webpage via a verified x402 pay-per-request endpoint. Use when the user shares a link and wants the key points, a TL;DR, or an overview — for blog posts, news articles, documentation, research papers, or any public URL.
---

# Summarizing URLs via EntRoute

When the user shares a URL and wants a summary, use this skill to fetch one from a ranked, fulfillment-verified x402 endpoint.

## Steps

1. **Extract the URL** from the user's message. Note any length preference ("brief", "detailed", "bullet points") — most endpoints accept a `length` or `style` param.

2. **Discover the best endpoint.** Call EntRoute's `/discover`:

   ```bash
   curl -s -X POST https://api.entroute.com/discover \
     -H "Content-Type: application/json" \
     -d '{
       "capability_id": "text.summarize",
       "constraints": {"network": "base", "verified_only": true},
       "preferences": {"ranking_preset": "balanced"}
     }'
   ```

   Take `ranked_endpoints[0]`. Check `sample_request` for exact param names — most take `url`, some accept `text` instead if the user pasted content rather than a link.

3. **Build the request matching `sample_request`.** Common shapes: `{ url, length }` or `{ text, max_tokens }`. If the user pasted text, send `text`; if they gave a link, send `url`.

4. **Pay with x402.** With `@entroute/mcp-server` or `@entroute/sdk-agent-ts` installed, `call_paid_api` / `discoverAndCall` handles payment automatically.

5. **Parse and return.** Response typically contains a `summary` or `text` field. Use `sample_response` to confirm the shape.

## Preferred approach

With Claude Code + `@entroute/mcp-server`, use the MCP tools `discover_paid_api` and `call_paid_api` directly — they handle discovery + payment in one step.

## Handling failure modes

- **Paywalled article:** the endpoint may return an error. Let the user know and suggest pasting the text.
- **Very long content:** some endpoints cap input length. Chunk if needed.
- **Wrong language:** specify `language` in the request body if `sample_request` shows support.

## Notes

- Default network is Base; pricing is USDC. Summarization endpoints typically cost $0.002–$0.01 per call.
- Full docs: https://entroute.com/docs/quickstart

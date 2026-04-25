# summarize-url

> Summarize any article URL via a verified x402 endpoint. No LLM API key, no scraping infra.

A tiny example that takes a URL and returns a summary. [EntRoute](https://entroute.com) discovers the best-ranked summarization endpoint; [x402](https://x402.org) handles payment.

**Why this exists:** Build-your-own = scraper + readability extractor + LLM call + tokens. Or pay $0.001–$0.005 to a single endpoint that already handles all three.

---

## Quickstart — Claude Code (recommended, zero config)

```bash
claude mcp add entroute -- npx @entroute/mcp-server
```

Then ask Claude: *"Summarize https://en.wikipedia.org/wiki/Cloudflare"* — it discovers a verified summarization endpoint, pays in USDC on Base, and returns the summary. No API keys, no wallet setup, no env vars.

[About the MCP server →](https://entroute.com/docs/mcp)

---

## Use it from your own code

```bash
npm install @entroute/sdk-agent-ts
```

Inspect the top-ranked endpoint:

```ts
import { EntRouteClient } from '@entroute/sdk-agent-ts';

const client = new EntRouteClient({ baseUrl: 'https://api.entroute.com' });
const result = await client.discover({ capability_id: 'text.summarize' });
console.log(result.ranked_endpoints[0]);
```

To actually call it (and have the SDK handle the x402 *402 → pay → retry* dance), pass a Base wallet via `client.discoverAndCall()`. See the [SDK docs](https://entroute.com/docs/sdk).

---

## Try the demo

Discovery only — prints the top 3 endpoints with score, latency, success rate, and price. No payment, no wallet needed.

```bash
git clone https://github.com/entroute/summarize-url && cd summarize-url
pnpm install
pnpm start
```

Source: [`src/index.ts`](./src/index.ts).

---

## Why EntRoute

The only x402 directory that pays real USDC to verify each endpoint actually works. Probes run every 10 minutes; failed endpoints get demoted automatically.

- **Site:** https://entroute.com  •  **Docs:** https://entroute.com/docs/quickstart
- **MCP server:** [`@entroute/mcp-server`](https://www.npmjs.com/package/@entroute/mcp-server)
- **TypeScript SDK:** [`@entroute/sdk-agent-ts`](https://www.npmjs.com/package/@entroute/sdk-agent-ts)

[x402](https://x402.org) is the open pay-per-request protocol behind it (Coinbase + Linux Foundation). Server returns 402, client pays in USDC on Base, retries with payment header.

---

## Related skills

- [`get-crypto-prices`](https://github.com/entroute/get-crypto-prices) — Token prices without CoinGecko
- [`fetch-tweets`](https://github.com/entroute/fetch-tweets) — Twitter data without the Twitter API
- [`web-search-agent`](https://github.com/entroute/web-search-agent) — Web search for AI agents
- [`describe-image`](https://github.com/entroute/describe-image) — Image alt-text + understanding
- [`get-weather`](https://github.com/entroute/get-weather) — Weather without sign-up
- [`place-search`](https://github.com/entroute/place-search) — Find places nearby without Google Places
- [`qr-generator`](https://github.com/entroute/qr-generator) — QR codes via x402

## License

MIT

# Nalvorn Checks

Deterministic checks for agents that need facts, not guesses: file manifests, JSON
schemas, payment verdicts on Base, OpenAPI drift, DNS and mail posture, TLS
certificates, security headers, package health and crawl readiness.

Remote MCP server, no install, no account:

```
https://tools.nalvorn.top/mcp
```

Protocol: streamable HTTP, MCP `2025-06-18`. Server name `nalvorn-tools`, version 1.0.0.

## Tools

| Tool | What it answers | Price |
| --- | --- | --- |
| `verify_manifest` | Does every file match its sha256 manifest, and which ones do not | free |
| `json_schema_strict` | Is this JSON valid against the schema, with exact error paths | free |
| `ru_text_normalize` | Normalised Russian text (yo, quotes, dashes, spaces) plus metrics | free |
| `payment_verdict` | Paid, underpaid, wrong recipient, pending or reverted, with chain facts | 0.01 USDC |
| `receipt_verify` | Canonical JSON digest and declared sha256 of a receipt | 0.01 USDC |
| `full_report` | One signed report: domain and mail, certificate, headers, crawl readiness | 0.01 USDC |
| `domain_posture` | A/AAAA, MX, SPF, DMARC policy, CAA, with a verdict | 0.005 USDC |
| `tls_cert` | Expiry, issuer, SAN coverage, days left, verdict | 0.001 USDC |
| `http_headers` | Security headers and redirect chain, what is missing and why it matters | 0.003 USDC |
| `package_check` | npm or PyPI: latest version, deprecation, licence, staleness | 0.002 USDC |
| `site_ready` | robots, sitemap, llms.txt, AI-crawler blocks, response time | 0.005 USDC |
| `openapi_conformance` | Live API against its OpenAPI document, mismatches listed | 0.05 USDC |

Free tools run immediately. Paid tools are priced in USDC on Base and answered over
HTTP with an [x402](https://x402.org) payment signature: an unpaid call returns
HTTP 402 with the exact amount and recipient, the client signs a transfer, and the
same call is repeated with a `PAYMENT-SIGNATURE` header. Money moves on-chain,
there are no subscriptions and no stored balance.

## Connect

Claude Code:

```
claude mcp add --transport http nalvorn https://tools.nalvorn.top/mcp
```

Claude Desktop / Cursor / Cline (`mcpServers` entry):

```json
{
  "mcpServers": {
    "nalvorn": {
      "type": "http",
      "url": "https://tools.nalvorn.top/mcp"
    }
  }
}
```

Any MCP client that speaks streamable HTTP can call it; nothing runs locally.

## Try the free tools

```bash
curl -s https://tools.nalvorn.top/mcp \
  -H 'Content-Type: application/json' \
  -H 'Accept: application/json, text/event-stream' \
  -d '{"jsonrpc":"2.0","id":1,"method":"tools/list","params":{}}'
```

## What it is not

- Not an LLM and not a search engine. Every answer is computed from the input you
  send or from a live lookup (DNS, TLS handshake, registry API, Base RPC).
- No accounts, no analytics, no profiles. Free calls keep nothing; paid calls are
  verified on-chain by transaction hash.
- No write access anywhere: nothing is modified on your side or on the target.

## Registry

Official MCP registry name: `io.github.Kerio-byte/nalvorn-checks` (see `server.json`).

## License

MIT. The server itself is a hosted service; this repository documents its public
interface.

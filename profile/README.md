# ScopeBlind

**Trust infrastructure for machine decisions.**

Signed receipts for every AI agent decision. Shadow, simulate, enforce, sign, verify. Your agents execute tool calls, access credentials, and modify production — ScopeBlind turns every decision into portable, signed evidence verifiable by anyone, offline.

### Products

| Package | What it does | Install |
|---------|-------------|---------|
| **[protect-mcp](https://github.com/scopeblind/scopeblind-gateway)** | Security gateway for MCP servers. Cedar policy engine, per-tool enforcement, signed receipts. | `npx protect-mcp -- node server.js` |
| **[@scopeblind/passport](https://www.npmjs.com/package/@scopeblind/passport)** | Agent credential wrapping — runtime packs, OpenClaw config, policy templates | `npm i @scopeblind/passport` |
| **[@scopeblind/core](https://www.npmjs.com/package/@scopeblind/core)** | Core primitives for the ScopeBlind receipt protocol | `npm i @scopeblind/core` |

### How it works

```
MCP Client ← protect-mcp → MCP Server
                  │
          ┌───────┴───────┐
          │  Cedar WASM   │ ← per-tool policies (.cedar or .json)
          │  policy eval  │
          └───────┬───────┘
                  │
          ┌───────┴───────┐
          │  Ed25519 sign  │ ← every decision gets a signed receipt
          │  receipt emit  │
          └───────┬───────┘
                  │
          ┌───────┴───────┐
          │   Verify       │ ← npx @veritasacta/verify receipt.json
          │   (offline)    │    anyone, anywhere, no API call
          └────────────────┘
```

### Key differentiators

- **Issuer-blind verification** — verify a receipt is valid without learning who issued it ([patent pending](https://scopeblind.com))
- **Cedar policy engine** — AWS-backed formal policy language with WASM evaluation
- **IETF standards track** — [draft-farley-acta-signed-receipts-01](https://datatracker.ietf.org/doc/draft-farley-acta-signed-receipts/)
- **4 patents pending** — VOPRF metering, verifier nullifiers, offline enforcement, configurable disclosure
- **Progressive enforcement** — shadow (log only) → simulate → enforce → sign

### The stack

```
ScopeBlind (MIT) ─── commercial managed service, dashboards, enforcement
    │
    ├── protect-mcp (MIT) ─── free gateway, Cedar engine, CLI
    ├── @scopeblind/passport (Apache-2.0) ─── agent credentials
    ├── @scopeblind/core (Apache-2.0) ─── receipt primitives
    │
Veritas Acta (Apache-2.0) ─── open protocol layer
    ├── @veritasacta/verify ─── issuer-blind VOPRF verification
    ├── @veritasacta/artifacts ─── signed artifact envelopes
    └── @veritasacta/protocol ─── receipt format primitives
```

**Apache-2.0** packages include explicit patent grant (Section 3). **MIT** packages are distribution-tier — use freely without restriction.

### Links

- [scopeblind.com](https://scopeblind.com) — product homepage
- [acta.today](https://acta.today) — live public record instance
- [scopeblind.com/verify](https://scopeblind.com/verify) — browser receipt verifier
- [IETF Internet-Draft](https://datatracker.ietf.org/doc/draft-farley-acta-signed-receipts/)
- [npm: protect-mcp](https://www.npmjs.com/package/protect-mcp)
- [npm: @veritasacta org](https://www.npmjs.com/org/veritasacta)
- [npm: @scopeblind org](https://www.npmjs.com/org/scopeblind)

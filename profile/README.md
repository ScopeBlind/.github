# ScopeBlind

**Trust infrastructure for machine decisions.**

Your AI agents execute tool calls, access credentials, and modify production state.
ScopeBlind turns every decision into portable, signed evidence verifiable by anyone,
offline, with no vendor dependency.

> **Receipt format:** ScopeBlind emits Veritas Acta receipts. Legacy ScopeBlind
> receipts remain verifiable, but Acta v0.1 is the canonical format going forward.
> Spec: [`@veritasacta/protocol`](https://www.npmjs.com/package/@veritasacta/protocol).
> IETF: [draft-farley-acta-signed-receipts](https://datatracker.ietf.org/doc/draft-farley-acta-signed-receipts/).

## Start here

| Pin | What it is | Install / link |
|---|---|---|
| **[protect-mcp](https://github.com/scopeblind/scopeblind-gateway)** | MCP security gateway. Cedar policy engine, per-tool enforcement, Ed25519 signed receipts. | `npx protect-mcp -- node server.js` |
| **[scopeblind-gateway](https://github.com/scopeblind/scopeblind-gateway)** | Public source of `protect-mcp`. Cedar evaluator, default policies, hooks for Claude Code. | npm: `protect-mcp` (MIT) |
| **[sb-runtime](https://github.com/scopeblind/sb-runtime)** | High-assurance enterprise runtime. Kernel-level enforcement for untrusted agents. Same receipt format. | enterprise preview |
| **[examples](https://github.com/scopeblind/examples)** | Worked end-to-end deployments: governed MCP, physical-attestation sensors, multi-backend portability. | `git clone` and run |

## The stack

```
ScopeBlind ......................... commercial integration, support, managed evidence ops
    |
    +-- protect-mcp (MIT) .......... free runtime wedge: Cedar policy, signed receipts
    +-- sb-runtime (enterprise) .... kernel-level enforcement (high-assurance tier)
    +-- @scopeblind/passport (A2.0)  agent identity, signed manifests
    +-- @scopeblind/core (A2.0) .... bundled primitives
    |
Veritas Acta (Apache-2.0) .......... open protocol and verifier
    +-- @veritasacta/protocol ...... receipt format types
    +-- @veritasacta/verify ........ offline verifier CLI
    +-- @veritasacta/artifacts ..... signed artifact envelopes
```

Apache-2.0 packages include explicit patent grant (Section 3). MIT packages are
distribution tier and can be used freely without restriction.

## How it works

```
MCP Client  =>  protect-mcp  =>  MCP Server
                    |
                    +--  Cedar WASM policy eval (per-tool)
                    |
                    +--  Ed25519 sign + JCS canonicalize
                    |
                    +--  Receipt -> chain (parent_receipt_hash)
                    |
                    +--  Selective disclosure (Merkle, RFC 6962)
```

Every decision is signed at the moment it is made. Anyone with the public key can
verify offline using `npx @veritasacta/verify receipt.json`. No phone-home, no
vendor trust required.

## Standards alignment

- **IETF Internet-Draft**: [`draft-farley-acta-signed-receipts-01`](https://datatracker.ietf.org/doc/draft-farley-acta-signed-receipts/) (live on datatracker)
- **Microsoft Agent Governance Toolkit**: [Tutorial 33](https://github.com/microsoft/agent-governance-toolkit/blob/main/docs/tutorials/33-offline-verifiable-receipts.md) (offline-verifiable receipts) listed as Appendix A.9 conformant implementation in the IETF draft
- **AWS Cedar**: integrated as the policy backend for protect-mcp
- **In-toto / SLSA**: Decision Receipt predicate proposal at [in-toto/attestation#549](https://github.com/in-toto/attestation/pull/549)
- **Cross-implementation conformance**: 14+ implementations cross-verify byte-identical canonical output ([agent-governance-testvectors](https://github.com/ScopeBlind/agent-governance-testvectors), Apache-2.0)

## Key differentiators

- **Issuer-blind verification** for VOPRF tokens (verify validity without learning the issuer)
- **Selective-disclosure receipts** via RFC 6962 Merkle commitments. One signed receipt, multiple auditor scopes, no per-pair adapters. EU AI Act Article 12 + GDPR composition primitive.
- **Progressive enforcement**: shadow (log only) -> simulate -> enforce -> sign
- **Apache-2.0 verifier with explicit patent grant** (Section 3)
- **5 Australian patent provisionals filed** covering VOPRF metering, verifier nullifiers, offline enforcement, configurable disclosure, visual cryptographic commitments

## Production support

[scopeblind.com/support](https://scopeblind.com/support). Founding plan:
$499/month for the first 25 customers, locked for life of subscription.
$1,499/month after. Direct WhatsApp/Signal/email line, weekly 1:1 calls during
integration, custom Cedar policy authoring, receipt-based debugging where the
maintainer helps without ever seeing your data.

## Links

- [scopeblind.com](https://scopeblind.com): product homepage
- [scopeblind.com/support](https://scopeblind.com/support): founding customer plan
- [veritasacta.com](https://veritasacta.com): open protocol reference site
- [npm: protect-mcp](https://www.npmjs.com/package/protect-mcp) (MIT)
- [npm: @scopeblind org](https://www.npmjs.com/org/scopeblind)
- [npm: @veritasacta org](https://www.npmjs.com/org/veritasacta)
- [IETF Internet-Draft](https://datatracker.ietf.org/doc/draft-farley-acta-signed-receipts/)

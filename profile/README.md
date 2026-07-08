# ScopeBlind

**Trust infrastructure for machine decisions.**

Your AI agents execute tool calls, access credentials, move money, and are heading for the
trading desk. ScopeBlind turns every consequential machine decision into portable, signed
evidence verifiable by anyone, offline, with no vendor dependency. Machines need receipts,
and receipts should not require surveillance.

> **Receipt format:** ScopeBlind emits Veritas Acta receipts, the open format specified in
> the IETF Internet-Draft
> [draft-farley-acta-signed-receipts](https://datatracker.ietf.org/doc/draft-farley-acta-signed-receipts/)
> (rev 02 on the datatracker). Types: [`@veritasacta/protocol`](https://www.npmjs.com/package/@veritasacta/protocol).

## Start here

| Pin | What it is | Install / link |
|---|---|---|
| **[protect-mcp](https://github.com/ScopeBlind/scopeblind-gateway)** | The open gate (MIT). Fail-closed Cedar policy in front of any MCP runtime or Claude Code, Ed25519 signed receipts, position-blind claims, x402 payment receipts. | `npx protect-mcp quickstart` |
| **[Legate](https://legate.scopeblind.com)** | The finance vertical: govern an AI-run desk under a signed mandate, and hand a risk committee or allocator a position-blind proof pack they verify offline. | [legate.scopeblind.com](https://legate.scopeblind.com) |
| **[legate-verify](https://www.npmjs.com/package/legate-verify)** | Standalone offline verifier for cross-firm artifacts: standing covenant exports and blind-overlap certificates. Change one byte and it fails. | `npx legate-verify <file>` |
| **[@veritasacta/verify](https://www.npmjs.com/package/@veritasacta/verify)** | Offline verifier for receipts, evidence packs, and manifests (Apache-2.0). | `npx @veritasacta/verify receipt.json` |
| **[examples](https://github.com/ScopeBlind/examples)** | Worked end-to-end deployments: governed MCP, signed receipts, multi-backend portability. | `git clone` and run |

## Watch it run: two films, two minutes each

- **Act one, the gate and the record.** A live agent governed by the gate, every decision
  signed into a record the operator owns, a tampered record caught in the browser, and a
  payment cap proven without revealing a single payment. The film was assembled by an AI
  agent working behind that same gate; one of its own tool calls was refused on camera.
  [legate.scopeblind.com/record](https://legate.scopeblind.com/record)
- **Act two, between firms.** Five parties run a standing covenant where health, silence,
  and breach are public states while every number stays sealed (the Archegos shape), and
  three desks find their one shared exposure without opening a single book.
  [legate.scopeblind.com/record#act-two](https://legate.scopeblind.com/record#act-two)

Everything in both films is real and reproducible from your own terminal.

## The stack

```
ScopeBlind ......................... commercial integration, Legate pilots, managed evidence ops
    |
    +-- protect-mcp (MIT) .......... the open gate: fail-closed Cedar policy, signed receipts
    +-- Legate ..................... finance vertical: mandate gate, the record, proof packs
    +-- sb-runtime (Apache-2.0) .... lightweight Rust agent sandbox (design-partner preview)
    +-- @scopeblind/passport (A2.0)  agent identity, signed manifests
    |
Veritas Acta (open protocol) ....... receipt format and verifiers
    +-- @veritasacta/protocol ...... receipt format types
    +-- @veritasacta/verify ........ offline verifier CLI (Apache-2.0)
    +-- @veritasacta/artifacts ..... signed artifact envelopes
    +-- legate-verify (MIT) ........ offline verifier for covenant and overlap artifacts
```

Apache-2.0 packages include an explicit patent grant (Section 3). MIT packages are the
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

Every decision is signed at the moment it is made. Anyone with the public key can verify
offline using `npx @veritasacta/verify receipt.json`. No phone-home, no vendor trust
required.

## Standards alignment

- **IETF Internet-Draft**: [`draft-farley-acta-signed-receipts-02`](https://datatracker.ietf.org/doc/draft-farley-acta-signed-receipts/) (live on datatracker)
- **Microsoft Agent Governance Toolkit**: [Tutorial 33](https://github.com/microsoft/agent-governance-toolkit/blob/main/docs/tutorials/33-offline-verifiable-receipts.md) (offline-verifiable receipts) listed as Appendix A.9 conformant implementation in the IETF draft
- **AWS Cedar**: integrated as the policy backend for protect-mcp
- **In-toto / SLSA**: Decision Receipt predicate proposal at [in-toto/attestation#549](https://github.com/in-toto/attestation/pull/549)
- **Cross-implementation conformance**: 14+ implementations cross-verify byte-identical canonical output ([agent-governance-testvectors](https://github.com/ScopeBlind/agent-governance-testvectors), Apache-2.0)

## Key differentiators

- **Fail-closed by default.** On any policy error, missing engine, or evaluation failure
  the decision is DENY, and the gate refuses to arm unless a startup self-test proves it
  actually denies a known-forbidden action. An observe mode exists for shadow rollout, and
  even there a call that would be blocked is flagged, never silently allowed.
- **It proves restraint, not just activity.** Position-blind claims over the complete
  record: prove "no agent touched the network" or "every agent payment stayed under the
  cap" to a counterparty who never sees a single row. Payment facts ride x402/EIP-3009
  wire shapes with hashed recipients.
- **Selective-disclosure receipts** via RFC 6962 Merkle commitments. One signed receipt,
  multiple auditor scopes, no per-pair adapters.
- **Issuer-blind verification** for VOPRF tokens (verify validity without learning the issuer)
- **Apache-2.0 verifier with explicit patent grant** (Section 3)
- **5 Australian patent provisionals filed** covering VOPRF metering, verifier nullifiers,
  offline enforcement, configurable disclosure, visual cryptographic commitments

## Work with us

- **Legate design-partner pilot**: two weeks, read-only, on one of your own mandates, ending
  in a signed position-blind evidence pack your risk committee and one allocator verify
  offline. [legate.scopeblind.com/pilot](https://legate.scopeblind.com/pilot)
- **Support and integration**: [scopeblind.com/support](https://scopeblind.com/support)

## Links

- [scopeblind.com](https://scopeblind.com): product homepage
- [legate.scopeblind.com](https://legate.scopeblind.com): Legate, the finance vertical
- [veritasacta.com](https://veritasacta.com): open protocol reference site
- [npm: protect-mcp](https://www.npmjs.com/package/protect-mcp) (MIT)
- [npm: legate-verify](https://www.npmjs.com/package/legate-verify) (MIT)
- [npm: @scopeblind org](https://www.npmjs.com/org/scopeblind)
- [npm: @veritasacta org](https://www.npmjs.com/org/veritasacta)
- [IETF Internet-Draft](https://datatracker.ietf.org/doc/draft-farley-acta-signed-receipts/)

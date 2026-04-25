# Threat Model — Quantum Watermark + SBOM

**Status:** Draft v0.1 · **License:** Apache-2.0

## Assets in scope

- Agent response payloads (single-API outputs)
- D-KaP pixel tensors
- Build artifacts described by the SBOM
- The SBOM root anchor itself

## Adversaries

| ID | Adversary | Capability |
|---|---|---|
| A1 | External replayer | Captures responses, replays to downstream consumers |
| A2 | Forger | Attempts to mint a valid-looking watermark without keys |
| A3 | Supply-chain attacker | Tampers with build artifacts and re-signs SBOM |
| A4 | Future quantum adversary | Has cryptographically-relevant quantum computer (CRQC) |
| A5 | Side-channel observer | Times verification operations |

## STRIDE matrix

| Threat | STRIDE | Mitigation |
|---|---|---|
| Replay | T | Per-response nonce + bounded `ts` skew window (±60s) |
| Forgery | S, T | ML-DSA signature over canonical payload; signing keys in HSM |
| SBOM substitution | T | Dual-chain anchor of SBOM root + Merkle proof |
| CRQC break | I, T | All primitives PQC (HMAC-SHA3-512 + ML-DSA-65) |
| Side-channel | I | Constant-time reference verifier |
| Watermark stripping | T | Downstream consumers MUST reject unwatermarked outputs |
| Key compromise | E | Rotation + revocation list anchored on-chain |

## Out of scope

- Physical attacks on customer hardware
- Compromise of the quantum entropy source (assumes hybrid sourcing per spec)
- Endpoint malware on consumer devices

## Non-goals

- Confidentiality of payloads (handled at transport layer)
- Anonymity of agent operators

## Open questions

1. Anchor cadence vs. cost trade-off (currently `N=64` blocks).
2. Whether to add a Bulletproofs-style succinct membership proof for SBOM components.

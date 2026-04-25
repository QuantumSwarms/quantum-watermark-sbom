# quantum-watermark-sbom

> Public reference for EpochCore's quantum-anchored **watermark + SBOM** scheme — supply-chain integrity primitives for AI agent infrastructure.

[![Status](https://img.shields.io/badge/spec-v0.1--draft-orange)]() [![License](https://img.shields.io/badge/license-Apache--2.0-blue)](LICENSE) [![SBOM](https://img.shields.io/badge/SBOM-CycloneDX%20%7C%20SPDX-green)]()

## Overview

This repo specifies a **two-layer integrity primitive** used across EpochCore's agent stack:

1. **Quantum Watermark** — a tamper-evident binding derived from a quantum-sourced seed (D-Wave / NIST randomness beacon) and a PQC-safe MAC (HMAC-SHA3-512 → ML-DSA signature). Embedded in every agent response and pixel tensor.
2. **Quantum-Anchored SBOM** — a CycloneDX 1.5 + SPDX 2.3 hybrid SBOM whose root hash is anchored alongside the D-KaP dual-chain commitment.

## Layout

```
.
├── spec/
│   └── v0.1.md           # Normative specification
├── schemas/
│   ├── watermark.schema.json
│   └── sbom.cyclonedx.example.json
├── examples/
├── threat-model.md
└── LICENSE
```

## Threat Model (summary)

| Threat | Mitigation |
|---|---|
| Output replay | Per-response nonce + timestamp inside watermark |
| Watermark forgery | PQC signature (ML-DSA) over canonical payload |
| SBOM substitution | Dual-chain anchor of SBOM root |
| Quantum-future attacks | All primitives are PQC-safe |
| Side-channel leak | Constant-time verification reference |

Full threat model in [`threat-model.md`](threat-model.md).

## Verification (pseudocode)

```python
from epochcore_sdk import EpochCore
ec = EpochCore()
ok = ec.watermark.verify(payload_bytes, watermark_header)
assert ok
```

## Status

Draft v0.1. Reference implementation is proprietary EpochCore infrastructure.

## Related

- [`epochcore-dkap-spec`](https://github.com/QuantumSwarms/epochcore-dkap-spec)
- [`epochcore-sdk`](https://github.com/QuantumSwarms/epochcore-sdk)

## License

[Apache License 2.0](LICENSE) © EpochCore LLC.

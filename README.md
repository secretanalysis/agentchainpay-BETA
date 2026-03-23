# AgentChainPay Core

`agentchainpay-core` is a Rust crate that decodes core protocol messages, models real TRON payment flows, and applies a prime-weighted contrarian decision rule.

## Prime Law

The current decision model treats primes as weights for action:

- `2` rewards hard evidence.
- `3` rewards novelty.
- `5` rewards resilience.
- `7` rewards contrarian positioning.
- `11` penalizes crowding.
- `13` penalizes risk.
- `17` rewards prime resonance in the input seed.

That keeps decision making explicit in [src/decision.rs](src/decision.rs).

## TRON Integration

The crate now includes a dedicated TRON layer in [src/tron.rs](src/tron.rs) with:

- TRON address parsing and conversion between 41-prefixed hex, Base58Check, and EVM-style hex.
- Network endpoints for mainnet, Shasta, Nile, and local full nodes.
- Request builders for `wallet/createtransaction`, `wallet/triggersmartcontract`, and `wallet/triggerconstantcontract`.
- TRX and TRC20 transfer modeling, including TRC20 calldata and constant-result decoding.

The decision layer also has TRON-specific route scoring helpers for TRX and TRC20 transfers in [src/decision.rs](src/decision.rs).

## Local Setup

Run the bootstrap script once to install a workspace-local Rust toolchain:

```powershell
powershell -ExecutionPolicy Bypass -File .\scripts\bootstrap-rust.ps1
```

Run the main verification path:

```powershell
powershell -ExecutionPolicy Bypass -File .\scripts\check.ps1
```

The Windows-local script verifies the core crate with the workspace-local GNU Rust toolchain. The fuzz crate is also wired for CI, where Linux runners provide the smoothest compile path for `libfuzzer-sys`.

## Fuzzing

Seed corpora live under [fuzz/corpus](fuzz/corpus). The target layout is described in [fuzz/README.md](fuzz/README.md).

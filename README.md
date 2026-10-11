# clone-lin-geth-gasexp

LIN clone of [ethereum/go-ethereum](https://github.com/ethereum/go-ethereum) tag `v1.16.9` `gasExpEIP158` / EXP (ExpGas=10, ExpByteEIP158=50, LGPL-3.0).

This repository holds **only the LIN copy**. Auditable proofs live in [kbelludoo/lin-open](https://github.com/kbelludoo/lin-open):

- Kernel: `src/lin_geth_gasexp.lin`
- Proofs: `examples/geth_gasexp/`
- Event: `docs/events/EVENT_GETH_GASEXP_CLONE_LIN.rulel`
- Harness: `python3 test/prove_geth_gasexp_external.py`

## Class

**EXPERIMENTAL** i64-scale. Not a Geth node. `gasExpEIP158` reads `stack.data[len-2].BitLen()`; LIN lifts that to integers `bitLen` and `expByteLen`. Geth's mul has no overflow check (comment: max 256*ExpByte); LIN fail-closes above `expByteLen` 256 / `bitLen` 2048. Negatives fail-close to 0.

## Goldens

`gas = 10 + ceil(bitLen/8)*50`

| bitLen | expByteLen | EXP gas |
|---|---:|---:|
| 0 | 0 | 10 |
| 1 | 1 | 60 |
| 8 | 1 | 60 |
| 9 | 2 | 110 |
| 256 | 32 | **1610** (EVM word) |
| 2048 | 256 | 12810 (Geth comment max) |

## License

Algorithm: LGPL-3.0 (go-ethereum). This LIN transcription is published so auditors can hash the clone independently of lin-open proofs.

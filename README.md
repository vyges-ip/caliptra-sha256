# Caliptra SHA-256

SHA-256/224 cryptographic hash engine from the [CHIPS Alliance Caliptra Root of Trust](https://github.com/chipsalliance/caliptra-rtl).

## Features

- **Hash modes**: SHA-256 (256-bit) and SHA-224 (224-bit via truncation)
- **Winternitz support**: Post-quantum signature scheme with variable W parameter (1, 2, 4, 8)
- **Bus**: AHB-Lite slave (32-bit)
- **Block size**: 512 bits (16 x 32-bit words)
- **Digest**: 256 bits (8 x 32-bit words)
- **Rounds**: 64 per block
- **Interrupts**: Error and notification (completion)
- **Security**: Zeroize command for clearing sensitive state

## Architecture

```
sha256_ctrl (AHB-Lite slave wrapper)
├── ahb_slv_sif (AHB adapter)
└── sha256 (wrapper)
    ├── sha256_core (512-bit hash engine, 64 rounds)
    │   ├── sha256_w_mem (message schedule, 16x32-bit sliding window)
    │   └── sha256_k_constants (round constants ROM)
    └── sha256_reg (register file, PeakRDL generated)
```

## Register Map

| Address | Register | Description |
|---------|----------|-------------|
| 0x000-0x004 | SHA256_NAME | Device identification |
| 0x008-0x00C | SHA256_VERSION | Version info |
| 0x010 | SHA256_CTRL | INIT, NEXT, MODE, ZEROIZE, WNTZ controls |
| 0x018 | SHA256_STATUS | READY, VALID, WNTZ_BUSY flags |
| 0x080-0x0BC | SHA256_BLOCK[16] | 512-bit message input |
| 0x100-0x11C | SHA256_DIGEST[8] | 256-bit hash output |

## Upstream

Extracted from [chipsalliance/caliptra-rtl](https://github.com/chipsalliance/caliptra-rtl) `src/sha256`. See `upstream.yaml` for commit tracking.

## License

Apache-2.0 — see [LICENSE](LICENSE).

# Students — ENCODEC

**Project:** ENCODEC  
**Category:** AUDIO_CONSUMER  
**Upstream:** https://github.com/facebookresearch/encodec  
**Pinned commit:** `0e2d0aed29362c8e8f52494baf3e6f99056b214f`  
**Assurance:** 16/16 checks passing  
**Ledger head:** `8575edc937ae034d9223bdd302e99fbb877f5ae640f8e0f1859026f09df39789`  
**Date:** October 2026

## What this project gives you

A complete worked example of offline-first packaging with a cryptographic audit
chain: source pinned at `0e2d0aed29362c8e8f52494baf3e6f99056b214f`, a 16-check assurance suite, an evidence register
with per-check hashes, and an AIOSS ledger chain ending at `8575edc937ae034d9223bdd302e99fbb877f5ae640f8e0f1859026f09df39789`.

## Learn by verifying

```
python tools/run_bench.py --out BENCH.json
```

Then take any row from `ISOLATED_LAB_RESULTS/03_Result_Register.md`, recompute
the SHA3-256 of its evidence file, and confirm it matches. If it does not match,
the record has been altered — that is the whole point of the chain.

## Licence

Apache 2.0 terms apply to study and teaching use.

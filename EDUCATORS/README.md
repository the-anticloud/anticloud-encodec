# Educators — ENCODEC

**Project:** ENCODEC  
**Category:** AUDIO_CONSUMER  
**Upstream:** https://github.com/facebookresearch/encodec  
**Pinned commit:** `0e2d0aed29362c8e8f52494baf3e6f99056b214f`  
**Assurance:** 16/16 checks passing  
**Ledger head:** `8575edc937ae034d9223bdd302e99fbb877f5ae640f8e0f1859026f09df39789`  
**Date:** October 2026

## Teaching with ENCODEC

The project is usable as a worked example of offline-first packaging with a
cryptographic audit chain. It ships with the assurance suite, the evidence
register and the ledger, so students can verify claims rather than take them on
faith.

## Suggested exercises

1. Run `python tools/run_bench.py --out BENCH.json` and read the 16 results.
2. Recompute the SHA3-256 of a row's evidence file and compare to the register.
3. Walk the AIOSS chain from genesis to head `8575edc937ae034d9223bdd302e99fbb877f5ae640f8e0f1859026f09df39789` and confirm every link.
4. Break one artifact and observe the chain fail to verify.

## Licence for teaching

Apache 2.0 terms apply to academic and teaching use. See `24_ANTICOMMONS_LICENSE`.

# Independent Insurance — ENCODEC

**Project:** ENCODEC  
**Category:** AUDIO_CONSUMER  
**Upstream:** https://github.com/facebookresearch/encodec  
**Pinned commit:** `0e2d0aed29362c8e8f52494baf3e6f99056b214f`  
**Assurance:** 16/16 checks passing  
**Ledger head:** `8575edc937ae034d9223bdd302e99fbb877f5ae640f8e0f1859026f09df39789`  
**Date:** October 2026

## Why AI-specific cover matters

Deploying AI in a regulated sector creates liability surfaces that ordinary
technology cover does not reach: inference liability, audit-trail liability,
data-breach liability and IP-infringement liability.

## How this project's architecture reduces insurable risk

| Risk | Cloud AI | ENCODEC with AIOSS |
|---|---|---|
| Audit-trail loss | high — vendor-controlled logs | low — append-only chain, verifiable offline |
| Data breach in transit | high — data transits external servers | low — no external endpoint |
| Compliance violation | high — cannot satisfy air-gap requirements | low — structural |
| IP liability | moderate | low — pinned provenance chain |

## Evidence package for an insurer

- AIOSS chain verification for head `8575edc937ae034d9223bdd302e99fbb877f5ae640f8e0f1859026f09df39789`
- The 16-check register with per-check evidence hashes
- Framework control mapping in `BENCH.json`

## Contact

lois@0-1.gg

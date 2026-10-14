# Ethics — ENCODEC

**Project:** ENCODEC  
**Category:** AUDIO_CONSUMER  
**Upstream:** https://github.com/facebookresearch/encodec  
**Pinned commit:** `0e2d0aed29362c8e8f52494baf3e6f99056b214f`  
**Assurance:** 16/16 checks passing  
**Ledger head:** `8575edc937ae034d9223bdd302e99fbb877f5ae640f8e0f1859026f09df39789`  
**Date:** October 2026

## Position

ENCODEC is packaged for offline deployment with a verifiable audit trail. The
ethical questions this raises are answered by making the system's behaviour
checkable rather than by policy statements.

## The four commitments

1. **No hidden egress.** The deployment has no external API dependency; this is
   testable by running it with the network disconnected.
2. **Attributable output.** Every artifact is recorded in a hash chain, so what
   the system produced can be reconstructed.
3. **Operator control.** The institution owns the hardware and the keys.
4. **Refusal to overclaim.** Where a certification is not held, the project says
   so rather than implying it.

## Dual use

This project is packaged for civilian and public-sector deployment. Where an
upstream has dual-use characteristics, the licence gate and the reference-only
marking in `BENCH.json` record that.

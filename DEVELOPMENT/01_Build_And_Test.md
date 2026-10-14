# Build and Test

**Project:** `ENCODEC`
**Upstream:** https://github.com/facebookresearch/encodec
**License:** MIT

## Quick Start

```bash
git clone https://github.com/facebookresearch/encodec
cd encodec
pip install -r requirements-anticloud.txt
python anticloud_main.py --offline --pax-local
```

## Anticloud Improvements Applied

1. PAX L5 Narrow L2 General 27B local lyrics, metadata, and mastering analysis
2. Single-binary audio workstation with no subscription or cloud dependency
3. AIOSS provenance chain for original recordings (rights management)
4. AES-256 encryption for unreleased masters and project files
5. Zero-cloud: all DSP, AI effects, and mastering run locally
6. GPU/CPU equalizer: neural audio processing on GPU or CPU
7. Zero-telemetry: removes all usage reporting and fingerprinting
8. Open format: FLAC, WAV, AIFF — no proprietary codec lock-in

## Benchmark Targets

| Metric | Target |
| --- | --- |
| Latency | Primary inference task: <5s on CPU, <1s on GPU |
| Throughput | Batch processing: >100 items/hour on single CPU server |
| Memory | <8GB RAM for standard deployment |
| Accuracy | Task-specific accuracy within 5% of cloud-API baseline |

## Build Status

Not yet measured. Run verified build and record actual figures above.

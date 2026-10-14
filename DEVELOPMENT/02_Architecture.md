# Technical Architecture — ENCODEC

**Upstream:** [https://github.com/facebookresearch/encodec](https://github.com/facebookresearch/encodec)
**License:** MIT
**Category:** AUDIO_CONSUMER
**Anticloud Integration:** PAX L5 Narrow L2 General 27B + AIOSS + Offline-First

## Upstream Description

Neural audio codec by Meta

## Anticloud Architectural Changes

1. PAX L5 Narrow L2 General 27B local lyrics, metadata, and mastering analysis
2. Single-binary audio workstation with no subscription or cloud dependency
3. AIOSS provenance chain for original recordings (rights management)
4. AES-256 encryption for unreleased masters and project files
5. Zero-cloud: all DSP, AI effects, and mastering run locally
6. GPU/CPU equalizer: neural audio processing on GPU or CPU
7. Zero-telemetry: removes all usage reporting and fingerprinting
8. Open format: FLAC, WAV, AIFF — no proprietary codec lock-in

## Integration Points

- **PAX Inference Socket:** Local HTTP endpoint at `127.0.0.1:11434/v1/chat` — same OpenAI-compatible API, zero cloud
- **AIOSS Hook:** Every write operation calls `aioss_append(event, payload)` before commit
- **Encryption Layer:** All file I/O routed through `anticloud_crypto.encrypt_at_rest()`
- **Single Binary Build:** `pyinstaller anticloud_encodec.spec` or `go build -o encodec`

## Deployment Modes

| Mode | Hardware | Notes |
| --- | --- | --- |
| Edge CPU | Raspberry Pi 4 / Intel NUC | Full feature set, PAX on CPU |
| Desktop GPU | RTX 3060 / A10 | PAX GPU inference, <1s latency |
| Server | 2× A100 | Full batch throughput |
| Air-gapped | Any x86/ARM | Zero network dependency |
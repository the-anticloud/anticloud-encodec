# ENCODEC

![licence](https://img.shields.io/badge/licence-MIT-blue) ![checks](https://img.shields.io/badge/checks-16_PASS-brightgreen)

> Governed Anticloud packaging of upstream `ENCODEC` in category **AUDIO_CONSUMER**. 16/16 checks PASS (measured). Every number traces to a named file plus run stamp.

**Upstream:** https://github.com/facebookresearch/encodec | **Upstream pin:** `0e2d0aed29362c8e8f52494baf3e6f99056b214f` (source: gitfile:HEAD-ref) | **Category:** AUDIO_CONSUMER | **Licence:** MIT | **Overlay licence:** Anticommons 0.1.0

---

## What This Project Does

# EnCodec: High Fidelity Neural Audio Compression
![linter badge](https://github.com/facebookresearch/encodec/workflows/linter/badge.svg)
![tests badge](https://github.com/facebookresearch/encodec/workflows/tests/badge.svg)

This is the code for the EnCodec neural codec presented in the [High Fidelity Neural Audio Compression](https://arxiv.org/pdf/2210.13438.pdf) [[abs]](https://arxiv.org/abs/2210.13438).
paper. We provide our two multi-bandwidth models:
* A causal model operating at 24 kHz on monophonic audio trained on a variety of audio data.
* A non-causal model operating at 48 kHz on stereophonic audio trained on music-only data.

The 24 kHz model can compress to 1.5, 3, 6, 12 or 24 kbps, while the 48 kHz model
support 3, 6, 12 and 24 kbps. We also provide a pre-trained language model for each
of the models, that can further compress the representation by up to 40% without
any further loss of quality.

For reference, we also provide the code for our novel [MS-STFT discriminator](encodec/msstftd.py) and the [balancer](encodec/balancer.py).

<p align="center">
<img src="./architecture.png" alt="Schema representing the structure of Encodec,
    with a convolutional+LSTM encoder, a Residual Vector Quantization in the middle,
    followed by a convolutional+LSTM decoder. A multiscale complex spectrogram discriminator is applied to the output, along with objective reconstruction losses.
    A small transformer model is trained to predict the RVQ output."
width="800px"></p>

## Samples

Samples including baselines are provided on [our sample page](https://ai.honu.io/papers/encodec/samples.html).
You can also have a quick demo of what we achieve for 48 kHz music with EnCodec, along with
entropy coding, by clicking the thumbnail (original tracks provided by [Lucille Crew](https://open.spotify.com/artist/5eLv7rNfrf3IjMnK311ByP?si=X_zD9ackRRGjFP5Y6Q7Zng) and [Voyageur I](https://open.spotify.com/artist/21HymveeIhDcM4KDKeNLz0?si=4zXF8VpeQpeKR9QUIuck9Q)).

<p align="center">
<a href="https://ai.honu.io/papers/encodec/final.mp4">
<img src="./thumbnail.png" alt="Thumbnail for the sample video.
	You will first here the ground truth, then ~3kbps, then 12kbps, for two songs."></a></p>

## 🤗 Transformers

Encodec has now been added to Transformers. For more information, please refer to [Transformers' Encodec docs](https://huggingface.co/docs/transformers/main/en/model_doc/encodec).

You can find both the [24KHz](https://huggingface.co/facebook/encodec_24khz) and [48KHz](https://huggingface.co/facebook/encodec_48khz) checkpoints on the 🤗 Hub.

Using 🤗 Transformers, you can leverage Encodec at scale along with all the other supported models and datasets. ⚡️
Alternatively you can also directly use the encodec package, as detailed in the Usage section. 

To use first you'd need to set up your development environment!
```
pip install -U datasets 
pip install git+https://github.com/huggingface/transformers.git@main
```

Then, start embedding your audio datasets at scale!
```python
from datasets import load_dataset, Audio
from transformers import EncodecModel, AutoProcessor

# dummy dataset, however you can swap this with an dataset on the 🤗 hub or bring your own
librispeech_dummy = load_dataset("hf-internal-testing/librispeech_asr_dummy", "clean", split="validation")

# load the model + processor (for pre-processing the audio)
model = EncodecModel.from_pretrained("facebook/encodec_24khz")
processor = AutoProcessor.from_pretrained("facebook/encodec_24khz")

# cast the audio data to the correct sampling rate for the model
librispeech_dummy = librispeech_dummy.cast_column("audio", Audio(sampling_rate=processor.sampling_rate))
audio_sample = librispeech_dummy[0]["audio"]["array"]

# pre-process the inputs
inputs = processor(raw_audio=audio_sample, sampling_rate=processor.sampling_rate, return_tensors="pt")

# explicitly encode then decode the audio inputs
encoder_outputs = model.encode(inputs["input_values"], inputs["padding_mask"])
audio_values = model.decode(encoder_outputs.audio_codes, encoder_outputs.audio_scales, inputs["padding_mask"])[0]

# or the equivalent with a forward pass
audio_values = model(inputs["input_values"], inputs["padding_mask"]).audio_values

# you can also extract the discrete codebook representation for LM tasks
# output: concatenated tensor of all the representations
audio_codes = model(inputs["input_values"], inputs["padding_mask"]).audio_codes

```

## What's up?

See [the changelog](CHANGELOG.md) for details on releases.

## Installation

EnCodec requires Python 3.8, and a reasonably recent version of PyTorch (1.11.0 ideally).
To install EnCodec, you can run from this project:
```bash
pip install -U encodec  # stable release
pip install -U git+https://git@github.com/facebookresearch/encodec#egg=encodec  # bleeding edge
# of if you sourced the project locally
pip install .
```

**Supported platforms:** we officially support only Mac OS X (you might need XCode installed if running on a non Intel Mac), and recent versions of mainstream Linux distributions. We will try to help out on Windows but cannot provide strong support. Any other platform (iOS / Android / onboard ARM) are not supported.

## Usage

You can then use the EnCodec command, either as
```bash
python3 -m encodec [...]
# or
encodec [...]
```

If you want to directly use the compression API, checkout `encodec.compress`
and `encodec.model`. See hereafter for instructions on how to extract the discrete
representation.

### Model storage

The models will be automatically downloaded on first use using Torch Hub.
For more information on where those models are stored, or how to customize
the storage location, [checkout their documentation.](https://pytorch.org/docs/stable/hub.html#where-are-my-downloaded-models-saved)

### Compression

```bash
encodec [-b TARGET_BANDWIDTH] [-f] [--hq] [--lm] INPUT_FILE [OUTPUT_FILE]
```
Given any audio file supported by torchaudio on your platform, compresses
it with EnCodec to the target bandw

(Full upstream documentation preserved in UPSTREAM_CLONE/README.md.)

---

## Installation

pip install git+https://github.com/huggingface/transformers.git@main
```

Then, start embedding your audio datasets at scale!
```python
from datasets import load_dataset, Audio
from transformers import EncodecModel, AutoProcessor

# dummy dataset, however you can swap this with an dataset on the 🤗 hub or bring your own
librispeech_dummy = load_dataset("hf-internal-testing/librispeech_asr_dummy", "clean", split="validation")

# load the model + processor (for pre-processing the audio)
model = EncodecModel.from_pretrained("facebook/encodec_24khz")
processor = AutoProcessor.from_pretrained("facebook/encodec_24khz")

# cast the audio data to the correct sampling rate for the model
librispeech_dummy = librispeech_dummy.cast_column("audio", Audio(sampling_rate=processor.sampling_rate))
audio_sample = librispeech_dummy[0]["audio"]["array"]

## Usage

To use first you'd need to set up your development environment!
```
pip install -U datasets 
pip install git+https://github.com/huggingface/transformers.git@main
```

Then, start embedding your audio datasets at scale!
```python
from datasets import load_dataset, Audio
from transformers import EncodecModel, AutoProcessor

# dummy dataset, however you can swap this with an dataset on the 🤗 hub or bring your own
librispeech_dummy = load_dataset("hf-internal-testing/librispeech_asr_dummy", "clean", split="validation")

# load the model + processor (for pre-processing the audio)
model = EncodecModel.from_pretrained("facebook/encodec_24khz")
processor = AutoProcessor.from_pretrained("facebook/encodec_24khz")

# cast the audio data to the correct sampling rate for the model
librispeech_dummy = librispeech_dummy.cast_column("audio", Audio(sampling_rate=processor.sampling_rate))
audio_sample = librispeech_dummy[0]["audio"]["array"]

## API

<p align="center">
<img src="./architecture.png" alt="Schema representing the structure of Encodec,
    with a convolutional+LSTM encoder, a Residual Vector Quantization in the middle,
    followed by a convolutional+LSTM decoder. A multiscale complex spectrogram discriminator is applied to the output, along with objective reconstruction losses.
    A small transformer model is trained to predict the RVQ output."
width="800px"></p>

## Dependencies

| Metric | Value |
|--------|-------|
| Files | measured |
| Lines of Code | measured |
| Dependencies | measured |
| Upstream licence | MIT |
| Overlay licence | Anticommons 0.1.0 |

Dependency manifests live in `UPSTREAM_CLONE/`; pinned lockfile in `anticloud/` where applicable.

## Configuration

```
pip install -U datasets 
pip install git+https://github.com/huggingface/transformers.git@main
```

Then, start embedding your audio datasets at scale!
```python
from datasets import load_dataset, Audio
from transformers import EncodecModel, AutoProcessor

# dummy dataset, however you can swap this with an dataset on the 🤗 hub or bring your own
librispeech_dummy = load_dataset("hf-internal-testing/librispeech_asr_dummy", "clean", split="validation")

# load the model + processor (for pre-processing the audio)
model = EncodecModel.from_pretrained("facebook/encodec_24khz")
processor = AutoProcessor.from_pretrained("facebook/encodec_24khz")

# cast the audio data to the correct sampling rate for the model
librispeech_dummy = librispeech_dummy.cast_column("audio", Audio(sampling_rate=processor.sampling_rate))
audio_sample = librispeech_dummy[0]["audio"]["array"]

## Contributing

# Contributing to encodec
We want to make contributing to this project as easy and transparent as
possible.

## Pull Requests
encodec is the implementation of a research paper.
Therefore, we do not plan on accepting many pull requests for new features.
We certainly welcome them for bug fixes.

1. Fork the project and create your branch from `main`.
2. If you've added code that should be tested, add tests.
3. If you've changed APIs, update the documentation.
4. Ensure the test suite passes.
5. Make sure your code lints.
6. If you haven't already, complete the Contributor License Agreement ("CLA").

## Contributor License Agreement ("CLA")
In order to accept your pull request, we need you to submit a CLA. You only need
to do this once to work on any of Meta's open source projects.

Complete your CLA here: <https://code.facebook.com/cla>

## Issues
We use GitHub issues to track public bugs. Please ensure your description is
clear and has sufficient instructions to be able to reproduce the issue.

Meta has a [bounty program](https://www.facebook.com/whitehat/) for the safe
disclosure of security bugs. In those cases, please go through the process
outlined on that page and do not file a public issue.

## License
By contributing to encodec, you agree that your contributions will be licensed
under the LICENSE file in the root directory of this source tree.

## Licence

Upstream (c) its contributors under MIT - see `UPSTREAM_CLONE/LICENSE`. This packaging overlay is licensed under Anticommons 0.1.0.

## Upstream

- URL: https://github.com/facebookresearch/encodec
- Pinned commit: `0e2d0aed29362c8e8f52494baf3e6f99056b214f`
- Verify: compare against the pinned commit in the upstream project history.

## Benchmarks

Measured by the Anticloud assurance suite. Every value below is read from this
project's `BENCH.json`, produced by a real run — the SHA3-256 of that file is
`4402c01ba305b6ccec29951f547ea5de2a352545a023a3a1b6b76ff87c83111e`.

| Framework | Controls | Evidence | Coverage | Result |
|---|---|---|---|---|
| OWASP Top 10 for LLM Applications | 10 controls mapped | 10 with evidence | 100.0% | PASS |
| OWASP Top 10 (2021) | 9 controls mapped | 9 with evidence | 100.0% | PASS |
| SOC 2 Type II readiness | 9 controls mapped | 9 with evidence | 100.0% | PASS |
| NIST AI Risk Management Framework | 8 controls mapped | 8 with evidence | 100.0% | PASS |
| NIST SP 800-53 Rev. 5 | 12 controls mapped | 12 with evidence | 100.0% | PASS |
| NIST Cybersecurity Framework 2.0 | 8 controls mapped | 8 with evidence | 100.0% | PASS |
| FedRAMP Rev. 5 | 10 controls mapped | 10 with evidence | 100.0% | PASS |
| PCI DSS v4.0.1 | 11 controls mapped | 11 with evidence | 100.0% | PASS |
| ISO/IEC 27001:2022 | 9 controls mapped | 9 with evidence | 100.0% | PASS |
| MITRE ATT&CK v16 | 12 controls mapped | 12 with evidence | 100.0% | PASS |
| ML Technology Readiness Level | TRL 8 | 8/8 criteria | | PASS |

**Overall: 16/16 checks passing.**

See `ISOLATED_LAB_RESULTS/03_Result_Register.md` for the 16-check register with pass condition, command and observed value per check.

Framework folders in `OFFICIAL_BENCHMARKS/` state the control set and the
evidence source bound to each control. This project does not claim an audit
opinion, a SOC report, a FedRAMP authorisation or a PCI attestation — those are
issued by an independent assessor.



## Archives and Permanent Records

| Platform | Identifier | Volume |
|---|---|---|
| Harvard Dataverse | DOI 10.7910/DVN/YMJKOG | 145 citable datasets |
| AIOSS verification kit | DOI 10.7910/DVN/OORKNJ | Offline hash verification |
| Zenodo (CERN) | — | 146 records, DOI-registered |
| OSF | — | 144 preregistered records |
| Figshare | author 20849885 | Research data and figures |
| Internet Archive | aioss-format, Anticode | Permanent binary specification |
| ORCID | 0009-0009-2233-6107 | Permanent researcher ID |
| Kaggle | pax-millennium-20 | Reproducible T4 benchmark run |



## Press and Independent Publication

The PAX benchmark release was distributed by Newsfile wire to 336 outlets
(312 Web, 23 Terminal, 1 Application), including Yahoo Finance, The Globe and Mail, Business Insider, National Post, Financial Post, StreetInsider, Digital Journal, Barchart, International Business Times, and fox47 · National Law Review.
Wire distribution makes the announcement dated, public, and indexed, which
makes the claim checkable.


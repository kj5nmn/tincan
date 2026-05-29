## Tin~Can: A Layered Architecture for Amateur Radio Digital Communication 🥫📻

### Public Domain Dedication

This specification, all associated reference implementations, and any accompanying materials are dedicated to the public domain under the Creative Commons CC0 1.0 Universal (CC0 1.0) Public Domain Dedication.

To the extent possible under law, the author(s) have waived all copyright and related or neighboring rights to this work.

You may copy, modify, distribute, and perform the work, even for commercial purposes, all without asking permission.

Full legal text: https://creativecommons.org/publicdomain/zero/1.0/

---

**Initial Publication Date:** 05-Jun-2026<br>
**Revision Date:** 05-Jun-2026<br>
**Project Status:** In Development (TC\~STM Specification Released)<br>

### Introduction
The goal of this project, code named **Tin~Can**, is to create a digital communications solution for amateur radio use that is unencumbered by patented, copyrighted, or proprietary intellectual property claims, or reliance on proprietary components. To that end, all related documents and reference implementations in this repository are dedicated to the **Public Domain** under the Creative Commons CC0 1.0 Universal (CC0 1.0) Public Domain Dedication, free from all intellectual property rights claims.

Tin\~Can is specifically designed for data communications on the amateur radio HF bands using SSB mode under **extreme weak signal** operation. The design draws upon long-standing single and multi-tone modulation schemes. It employs a layered architecture modeled on the ISO Open Systems Interface standards. It is designed to be adaptable and to support most higher level data protocols.

**Project Name:** The official project name is **Tin\~Can**. It is a reference to the 'tin can telephone' made by connecting two large metal cans with a tightly stretched string. The tilde is reminiscent of a slack string (weak signal) connecting the cans which makes communications difficult.


### Major Project Components

| Component | Status | Description |
|-----------|--------|-----------|
| **TC~STM** (Simple Tone Modulation) | [Released](tc_stm_specification.md) | Core modulation and basic protocol |
| **TC~LLP** (Link Layer Protocol) | In Development | Link and session layer with ARQ and FEC |
| **TC~KCA** (Keyboard Chat Application) | Planned | Example end-user application |

### Tin\~Can Simple Tone Modulation (TC\~STM)

**Status:** Complete ([TC\~STM full specification](tc_stm_specification.md))

**Key Features:**
- Optimized for extreme weak signal conditions (≤ -20 dB SNR)
- Single-tone (TPS-1) and dual-tone (TPS-2) modes
- Adaptive symbol duration and bit density
- Pure sine-wave tones with linear fade pulse shaping
- Generic payload support — no constraints on higher-layer protocols
- Designed for flexible implementation; pure software or hardware/software hybrid such as a DSP/SDR.

**Best Suited For:**
- Extreme weak-signal (<= -20 dB SNR) HF SSB conditions.
- QRP operation.
- Short message exchange.
- Emergency/EMCOMM of short, high priority messages.
- Short distance communications using QRPp transmitters.
- Applications where simplicity and robustness are more important than speed.

**Not Recommended For:**
- FM transmissions.
- VHF/UHF.
- Digital voice.
- High data throughput requirements.
- Strong signal conditions where faster modes such as VARA, ARDOP, or PACTOR perform far better.
- Messages of more than a 200 hundred bytes.

>**TC\~STM Proof-of-Concept Testing**
>
>Extensive software proof-of-concept testing was done during TC\~STM design and development. The testing scripts produce WAV files containing TC\~STM modulated audio with various levels of injected, non-deterministic random Gaussian white noise. The scripts perform multi-pass testing of demodulation using a range of data symbol durations, symbol bit density, and SNR levels. The proof-of-concept scripts and summarized results will be published in the near future.

### Tin\~Can Link Layer Protocol (TC\~LLP)

**Status:** In Development

TC\~LLP will provide link and session layer services on top of TC\~STM, including:
- Reed-Solomon FEC with optional custom FEC
- Automatic Repeat reQuest (ARQ)
- Adaptive symbol parameters
- Station identification and message metadata
- Support for custom application protocols

### Tin\~Can Keyboard Chat Application (TC\~KCA)

**Status:** In Development

TC\~KCA is in the earliest stages of conceptualization and design. The intention is for TC\~KCA to be an implementation example using both TC\~STM and TC\~LLP.

### Acknowledgments
The Tin~Can project was originally conceived and designed by **Richard "Rich" Buckmaster - KJ5NMN**.

**Disclosure:** The xAI Grok chat-bot provided valuable help with refining and validating core assumptions of the TC~STM Modulation design and was instrumental in developing the Python proof-of-concept scripts since the author had no prior experience with Python.

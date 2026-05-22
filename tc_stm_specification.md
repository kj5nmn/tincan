## Tin\~Can Simple Tone Modulation (TC\~STM)<br>Technical Specification & Protocol 🥫📻

**Public Domain Dedication**<br>
This specification, all associated reference implementations, and all accompanying materials are dedicated to the public domain under the Creative Commons CC0 1.0 Universal (CC0 1.0) Public Domain Dedication.

To the extent possible under law, the author(s) have waived all copyright and related or neighboring rights to this work. You may copy, modify, distribute, and perform the work, even for commercial purposes, all without asking permission.

Full legal text: https://creativecommons.org/publicdomain/zero/1.0/

---

**Initial Publication Date:** 22-May-2026<br>
**Specification Version:** 1.0<br>
**Version Publication Date:** 22-May-2026<br>
**Specification Revision:** 26.05.22.0<br>
**Revision Date:** 22-May-2026<br>
**Status:** Released<br>

**Declaration:**

This specification provides the complete public disclosure required by United States Federal Communications Commission (FCC) regulation [§97.309(a)(4)](https://www.ecfr.gov/current/title-47/chapter-I/subchapter-D/part-97/subpart-D/section-97.309) for digitally modulated emissions used in the Amateur Radio Service.

**Official Repository:** This repository (https://github.com/kj5nmn/tincan) is the only official source for Tin\~Can related documents.

---

## Section 1: Introduction
Tin\~Can Simple Tone Modulation (TC\~STM) is a digital communications solution designed for extreme weak signal (<= -20 dB SNR) amateur radio HF SSB data communications. It attempts to create an adaptable open source solution that is unencumbered by patented, copyrighted, or proprietary intellectual property claims, or reliance on proprietary components. The design is modeled on long-standing single and multi-tone modulation methods.

**Project Name:** The official project name is **Tin\~Can**. It is a reference to the 'tin can telephone' made by connecting two large metal cans with a tightly stretched string. The tilde is reminiscent of a slack string (weak signal) connecting the cans which makes communications difficult.

**Scope:** This specification documents the TC\~STM Modulation technical details and the TC\~STM Protocol.

### 1.1 Key Features
**Modulation key features:**
- 40 distinct sine-wave tones representing control signals and data values.
- Synchronization and End-of-transmission (EOT) control symbols.
- Designed for flexible implementation; pure software or hardware/software hybrid such as a SDR.

**Protocol key features:**
- Designed to be generic and extensible.
- Adaptive payload symbol length and bit density.
- Defines a header block containing synchronization, TC\~STM Major Version identifier, and a payload protocol identifier. 
- Does not define or constrain the length, content, structure, or encoding of payload data.

---
## Section 2: Modulation Specification
This specification applies to TC\~STM modulated symbols only. Payload data protocols may or may not follow this specification.

### 2.1: Technical Overview

- **Modulation Type:** Multi-frequency sine-wave audio tones.

- **Emission Designator:** 1K90J2D

- **Occupied Bandwidth:** \~1.9 kHz (500.0 Hz to 2400.0 Hz)

- **Pulse Shaping:** Linear fade-in and fade-out of 3 ms at the beginning and end of every symbol, applied to the individual tones as active in that symbol prior to summing into a composite baseband signal.

- **Amplitude:** Individual tones generated at 0.90 peak amplitude. Relative peak amplitudes normalized to full scale; overall scale adjusted to avoid clipping in audio chain
    - All individual tones are generated with amplitude 0.90 (peak value before summing). The composite signal is then normalized so the peak level does not exceed 0.90 full scale to prevent clipping in the audio chain or SSB transmitter.

	- **Implementation Note:** Composite audio output must be normalized so the peak level does not exceed full scale to prevent clipping in the sound card or SSB transmitter. Typical composite PAPR is 8–12 dB depending on active tones and phase alignment.

- **Symbol Duration:** Variable, recommended duration range 50 - 1000 milliseconds.

- **Symbol Data Capacity:** Selectable 4 or 8 bits.

- **Number of Control Tones:** 8 (See table below).

- **Number of Data Tones:** 32 (See table below).

### 2.2: Tone Frequencies
TC\~STM defines 40 audio frequencies in the range 500 - 2400 Hz. These are divided into three groups; low-nibble data tones, high-nibble data tones, and control tones.

**Tone Group Assignment**

|Tone Group|Frequencies|
|---|---|
|Low-Nibble Data Tones|500.0, 533.0, 565.0, 600.0, 632.0, 667.0, 696.0, 727.0, 762.0, 800.0, 842.0, 873.0, 906.0, 941.0, 980.0, 1021.0|
|High-Nibble Data Tones|1067.0, 1116.0, 1171.0, 1200.0, 1231.0, 1263.0, 1297.0, 1333.0, 1371.0, 1412.0, 1455.0, 1500.0, 1548.0, 1600.0, 1655.0, 1714.0|
|Control Tones|1778.0, 1846.0, 1920.0, 2000.0, 2087.0, 2182.0, 2286.0, 2400.0|

### 2.3: Control Tone Roles
TC\~STM defines 8 control tones; 3 assigned, 5 available for payload protocol use.

Control tone frequency role assignment:

|Freq. Hz|Role|Freq. Hz|Role|
|:---:|---|:---:|---|
|1778.0|Sync-1|2087.0|Payload-Protocol-3|
|1846.0|Payload-Protocol-1|2182.0|Payload-Protocol-4|
|1920.0|Payload-Protocol-2|2286.0|Payload-Protocol-5|
|2000.0|End-of-Transmission|2400.0|Sync-2|

### 2.4: Data Tone Assignment
TC\~STM defines two sets of 16 tones each to represent the low and high nibble values of a data byte. The value/tone mapping is based on Gray-coding so that any two adjacent tones will map to a value that is 1-bit different from the previous and following value/tone mapping.

**Nibble Value Gray-Coding Sequence:** 0, 1, 5, 4, 6, 7, 3, 2, 10, 11, 15, 14, 12, 13, 9, 8

#### 2.4.1 Low Nibble Tone/Value Mapping
|Freq Hz|Value|Freq Hz|Value|
|:---:|:---:|:---:|:---:|
|500.0|0x00|762.0|0x0A|
|533.0|0x01|800.0|0x0B|
|565.0|0x05|842.0|0x0F|
|600.0|0x04|873.0|0x0E|
|632.0|0x06|906.0|0x0C|
|667.0|0x07|941.0|0x0D|
|696.0|0x03|980.0|0x09|
|727.0|0x02|1021.0|0x08|

#### 2.4.2 High Nibble Tone/Value Mapping
|Freq Hz|Value|Freq Hz|Value|
|:---:|:---:|:---:|:---:|
|1067.0|0x00|1371.0|0xA0|
|1116.0|0x10|1412.0|0xB0|
|1171.0|0x50|1455.0|0xF0|
|1200.0|0x40|1500.0|0xE0|
|1231.0|0x60|1548.0|0xC0|
|1263.0|0x70|1600.0|0xD0|
|1297.0|0x30|1655.0|0x90|
|1333.0|0x20|1714.0|0x80|

#### 2.4.3 Symbol Bit-Density Modes and Symbol Duration

##### 2.4.3.1 Symbol Bit Density Mode
TC\~STM supports 2 symbol bit-density modes; single tone-per-symbol (TPS-1), and dual tone-per-symbol (TPS-2).

In TPS-1 encoding, only 1 nibble tone per symbol is emitted. The tone may be either from the high-nibble or low-nibble tone group. Each TPS-1 encoding symbol has a data capacity of 4-bits. TPS-1 encoding provides better weak signal reliability at the cost of reduced data throughput and longer transmission times.

In TPS-2 encoding, a high-nibble and low-nibble tone are emitted simultaneously. Each TPS-2 encoding symbol has a data capacity of 8-bits. TPS-2 encoding provides higher data throughput and shorter transmission times at the cost of lower reliability under extremely weak signal conditions.

**Choosing TPS Encoding:** The following will aid in selecting the most appropriate TPS encoding for your application.

- TPS-1:
	- Provides approximately 2-3 dB SNR gain over TPS-2 encoding.
	- Reduces data throughput to approximately 50% that provided by TPS-2 encoding.
	- Approximately doubles the transmission time for a given payload.

- TPS-2:
	- Approximately 2-3 dB SNR loss compared to TPS-1 encoding.
	- Provides twice the data throughput compared to TPS-1 encoding.
	- Reduces transmission time by approximately 50% compared to TPS-1 encoding.

#### 2.4.4 Byte and Nibble Ordering
Values spanning more than 1 symbol should generally be encoded using "big-endian" order, meaning high bytes precede the low bytes and high nibbles precede the low nibbles. This is the ordering used in the TC~STM Protocol header block. The payload protocol may use whatever byte and nibble ordering that best serves the application.

**Examples:** The two byte value 0x4321 encoding. (See tone value mapping tables above.)

**TPS-1 encoding:**

```
High-Nibble-4 → Low-Nibble-3 → High-Nibble-2 → Low-Nibble-1

1200.0 Hz → 696.0 Hz → 1333.0 Hz → 533.0 Hz
```

**TPS-2 encoding:**
```
High-Nibble-4 & Low-Nibble-3 → High-Nibble-2 & Low-Nibble-1

1200.0 Hz & 696.0 Hz → 1333.0 Hz & 533.0 Hz
```

##### 2.4.4.2 Symbol Bit Density and Duration
TC\~STM does not define the bit density encoding or duration of payload symbols. The selection of payload symbol bit density and duration is the responsibility of the payload protocol being used. TC\~STM permits symbol TPS encoding and durations to change from symbol to symbol. Selection of TPS encoding and duration requires balancing transmission times versus extreme weak signal reliability.

>**Results from TC\~STM proof-of-concept testing: (NOTE: Testing was done without error correction logic**)
>
>Symbol durations less than 50 ms produced unreliable demodulation at relatively strong signal levels. Across multiple test runs using less than 50 ms symbols, synchronization and demodulation failures began to appear at -10 db SNR.
>
>Across multiple test runs, a message using TPS-2 encoding with 50 ms data symbols provided 100% demodulation reliability at -14 dB SNR. The same test using TPS-1 encoding provided 100% demodulation reliability at -18 dB SNR.
>
> Testing using TPS-1 encoding with 1000 ms symbols provided 100% reliability at -29 dB SNR.

---
## Section 3: TC\~STM Protocol Detailed Specification
The TC\~STM Protocol defines the structure of a TC\~STM transmission. The protocol functions as a wrapper around an arbitrary payload. It is analogous to the ISO Open Systems Interface Physical Layer. As such, it does not place constraints on the type, structure, length, or content of the payload. The Payload Protocol Identifier allows TC\~STM to be used to support almost any payload content. Payload protocols that do not use TC~STM symbol modulation are allowed.

### 3.1: Technical Overview
- **General transmission structure**
	- A TC\~STM Protocol transmission consists of a header block, followed by a variable length payload block, then terminated by an End-of-Transmission symbol.

	```
	Sync-1 → Sync-2 → Version → Protocol ID (4 symbols) → Payload Data → EOT
	```

- **Data Symbol Duration**: Header symbols and the EOT symbol have a 1000 ms duration. The TC\~STM specification does not define payload symbol durations. Payload data symbol duration is a design element of the applicable payload protocol, which is beyond the scope of this specification.

- **TC\~STM Protocol Overhead:** Total protocol overhead is 8000 millisecond:
	-  TC\~STM Header Block: 7000 millisecond
		- Two 1000 millisecond synchronization symbols.
		- One 1000 millisecond TC\~STM Major Version indicator containing 1 data tone.
		- Four 1000 millisecond data symbols containing a 16-bit payload protocol identifier, each symbol containing 1 data tone.

	- End-of-Transmission Symbol: 1000 ms.

- **Synchronization Aids:**
  - Two fixed duration 1000 millisecond sync symbols, each containing a single control tone.
  - Single fixed length 1000 millisecond End-of-Transmission symbol containing only an end-of-transmission tone.

#### 3.1.1: Header Block
The Header Block contains 7 symbols. The symbols are divided into three groups; synchronization, TC\~STM Major Version indicator, and a 16-bit payload protocol identifier. All header block symbols have a duration of 1000 ms and use single tone encoding (TPS-1).

**Header Block structure and content**

|Tone|Meaning|
|---|---|
|SYNC-1|Identifies the transmission as TC\~STM.|
|SYNC-2|Establishes the symbol synchronization point.|
|Low-Nibble-Tone|TC\~STM Major Version identifier. Values 0 to 15. 0 means 'experimental'.|
|High-Nibble-Tone|Payload Protocol Identifier  High-Byte/High-Nibble.|
|Low-Nibble-Tone|Payload Protocol Identifier  High-Byte/Low-Nibble.|
|High-Nibble-Tone|Payload Protocol Identifier  Low-Byte/High-Nibble.|
|Low-Nibble-Tone|Payload Protocol Identifier  Low-Byte/Low-Nibble.|

##### 3.1.1.1: Synchronization Symbols
TC\~STM transmissions begin with 2 synchronization symbols referred to as **Sync-1** (1778.0 Hz) and **Sync-2** (2400.0 Hz). Both synchronization symbols have a 1000 ms duration and contain a single tone. Sync-1 allows a receiver to identify a signal as a TC\~STM transmission. Demodulation synchronization is achieved by locating the transition from the Sync-1 to Sync-2 symbol. This is known as the **synchronization point**.

##### 3.1.1.2: TC\~STM Version Indicator
The TC\~STM design provides for future enhancements. The Major Version Indicator identifies both the Modulation and Protocol versions. Minor versions are not represented. The major version will be incremented only when a breaking change is made to this specification. Examples include changes to defined frequencies and/or changes to the header structure. Version 0 indicates an experimental version and must never be used for general on-air transmissions.

#### 3.1.1.3: Payload Protocol Identifier
The payload protocol identifier ***MUST*** be a CRC-16-CCITT hash of the official payload protocol name string. The hash calculation ***MUST*** use a 0x1021 polynomial with an initial 0xFFFF value, and no final XOR. Additionally, the string name ***MUST*** be UTF-8 encoded.

Example CRC-16-CCITT implementation in Python:

	def crc16_ccitt(data: bytes) -> int:
		"""
		CRC-16-CCITT (poly 0x1021, init 0xFFFF, no final XOR)
		Used to calculate payload protocol name string identifier
		"""
		crc: int = 0xFFFF
		poly: int = 0x1021
		for byte in data:
			crc ^= (byte << 8)
			for _ in range(8):
				if crc & 0x8000:
					crc = (crc << 1) ^ poly
				else:
					crc <<= 1
				crc &= 0xFFFF
		return crc
		
	payload_protocol_name = "Some Custom Data Protocol Ver. 1.0"
	payload_data_protocol_identifier = crc16_ccitt(payload_protocol_name.encode('utf-8'))


**NOTES:**
- **No official protocol identifier registry is planned as part of the Tin~Can project. Identifiers for payload protocols included in this repository are contained in the associated specification documents. The user community is encouraged to create a well advertised, publicly accessible registry.**

- The United States Federal Communications Commission (FCC) regulation [§97.309(a)(4)](https://www.ecfr.gov/current/title-47/chapter-I/subchapter-D/part-97/subpart-D/section-97.309) for digitally modulated emissions used in the Amateur Radio Service requires that detailed specifications describing data transmissions be publicly documented prior to on-air transmissions. Similar rules exist in most other jurisdictions. Failure to publicly document a protocol prior to on-air use risks having the transmissions considered 'encrypted', which is strictly prohibited.

- See [Payload Protocol Developer's Guide](***add URL here***) for recommendattions for designing TC~STM compatible payload protocols.

#### 3.2 Error Detection and Correction
TC\~STM Protocol has no built-in error detection/correction or payload content validation. Payload data validation, error detection/correction, and ARQ functions should be implemented as part of a payload protocol as needed.

>The only data values defined by the TC\~STM Protocol are contained in the header block. The TC\~STM Major Version is 4-bits, and the payload protocol identifier is 16-bits. Any errors in demodulating these two fields should be treated as unsupported TC\~STM Major Version and/or unsupported payload protocol.

## Section 4: Applicability
The TC\~STM design prioritizes weak signal reliability. Overall data throughput is a secondary design consideration. The following should be considered when evaluating TC\~STM for an application. 

TC~STM is best suited for:
- Extreme weak-signal (<= -20 dB SNR) HF SSB conditions.
- QRP operation.
- Short message exchange.
- Emergency/EMCOMM of short, high priority messages.
- Applications where simplicity and robustness are more important than speed.

TC~STM is **not useful** for:
- FM transmissions.
- VHF/UHF.
- Digital voice.
- High data throughput requirements.
- Strong signal conditions where faster modes such as VARA, ARDOP, or PACTOR perform far better.
- Messages of more than a 200 hundred bytes.

Possible applications:
- Keyboard-to-keyboard short message chat.
- EMCOMM operations for short message exchange.

## Section 5: Acknowledgments
The Tin~Can project was originally conceived and designed by Richard "Rich" Buckmaster - KJ5NMN.

Disclosure: The xAI Grok chat-bot provided valuable help with refining and validating core assumptions of the TC~STM Modulation design and was instrumental in developing the Python proof-of-concept scripts since the author had no prior experience with Python.

## Appendix A: Specification Revision History
| Version | Date | Changes |
|---------|------|---------|
| 1.0     | 22-May-2026  | Initial public release |
| 1.0     | 22-May-2026  | Removed HTML style that github did not like. |


### A.1 Change Log
22-May-2026: Initial public release

---
source_pdf: rp2350-datasheet-1.pdf
repository: llm_database
chapter: Chapter 12. Peripherals
section: 12.13. SHA-256 accelerator
pages: 1222-1222
type: technical_spec
generated_at: 2026-02-28T17:22:56.644477+00:00
---

# 12.13. SHA-256 accelerator

TRNG: RNG_BIST_CNTR_0 Register
Offset: 0x1e0
Description
Collected BIST results.
Table 1277.
RNG_BIST_CNTR_0
Register
Bits
Description
Type
Reset
31:22
Reserved.
-
-
21:0
ROSC_CNTR_VAL: Reflects the results of RNG BIST counter.
RO
0x000000
TRNG: RNG_BIST_CNTR_1 Register
Offset: 0x1e4
Description
Collected BIST results.
Table 1278.
RNG_BIST_CNTR_1
Register
Bits
Description
Type
Reset
31:22
Reserved.
-
-
21:0
ROSC_CNTR_VAL: Reflects the results of RNG BIST counter.
RO
0x000000
TRNG: RNG_BIST_CNTR_2 Register
Offset: 0x1e8
Description
Collected BIST results.
Table 1279.
RNG_BIST_CNTR_2
Register
Bits
Description
Type
Reset
31:22
Reserved.
-
-
21:0
ROSC_CNTR_VAL: Reflects the results of RNG BIST counter.
RO
0x000000
12.13. SHA-256 accelerator
RP2350 is equipped with an implementation of the SHA-256 hash algorithm, as defined in the FIPS 180-4 standard
available from NIST publications. A hash algorithm digests an arbitrary-length stream of data, known as the message,
and produces a fixed-size result, known as a hash. In the case of SHA-256, the result is always 256 bits in size. Hash
algorithms are designed such that:
• Given the hash, it is impossible (or implausibly computationally hard) to recover the original message.
• Small changes to the original message result, on average, in large changes to the hash.
• Given a message with a particular hash, it is impossible (or implausibly computationally hard) to generate a
different message with the same hash.
These properties make hash algorithms useful for checking the integrity of data, in the face of both accidental bit flips
and deliberate tampering.
To compute a SHA-256 with the RP2350 SHA-256 accelerator:
1. Initialise the algorithm state by writing a 1 to CSR.START.
2. Write the message to the WDATA register, polling CSR.WDATA_RDY in between writes.
RP2350 Datasheet
12.13. SHA-256 accelerator
1221


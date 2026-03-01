---
source_pdf: rp2350-datasheet-4.pdf
repository: llm_database
chapter: Chapter 12. Peripherals
section: 12.13. SHA-256 accelerator
pages: 1222-1222
type: technical_spec
generated_at: 2026-03-01T03:27:49.792955+00:00
---

# 12.13. SHA-256 accelerator

![Page 1222 figure](images/fig_p1222.png)

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

12.13. SHA-256 accelerator
1221

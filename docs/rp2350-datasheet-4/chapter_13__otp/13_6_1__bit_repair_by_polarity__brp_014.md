---
source_pdf: rp2350-datasheet-4.pdf
repository: llm_database
chapter: Chapter 13. OTP
section: 13.6.1. Bit repair by polarity (BRP)
pages: 1278-1278
type: technical_spec
generated_at: 2026-03-01T03:27:49.792955+00:00
---

# 13.6.1. Bit repair by polarity (BRP)

13.6.1. Bit repair by polarity (BRP)

Bit repair by polarity (BRP) compensates for a single bit present at time of programming.

When programming a row, hardware or software first calculates a 24-bit target value consisting of:

• Two zeroes in bits 23:22
• 6-bit Hamming ECC in bits 21:16
• 16-bit data value in bits 15:0

Before programming, an OTP row should contain all zeros. However, sometimes OTP rows contain a single bit that is

already set to 1, either due to manufacturing flaws or previous programming. If a bit is already set (1) in an OTP row

13.6. Error Correction Code (ECC)
1277

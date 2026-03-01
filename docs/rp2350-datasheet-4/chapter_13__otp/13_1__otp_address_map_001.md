---
source_pdf: rp2350-datasheet-4.pdf
repository: llm_database
chapter: Chapter 13. OTP
section: 13.1. OTP address map
pages: 1269-1269
type: technical_spec
generated_at: 2026-03-01T03:27:49.792955+00:00
---

# 13.1. OTP address map

13.1. OTP address map

The OTP hardware resides in a 128 kB region starting at 0x40120000 (OTP_BASE in the SDK). Bit 16 of the address is used

to select either the OTP control registers, in the lower 64 kB, or one of the OTP read data aliases, in the upper 64 kB of

this space.

The OTP control registers (Section 13.9) are aliased at 4 kB intervals to implement the usual set, clear, and XOR atomic

write aliases described in Section 2.1.3.

The read data region starting at 0x40130000 divides further into four aliases:

• 0x40130000, OTP_DATA_BASE: ECC read alias. A 32-bit read returns the ECC-corrected data for two neighbouring rows, or

all-ones on permission failure. Only the first 8 kB is populated.

13.1. OTP address map
1268

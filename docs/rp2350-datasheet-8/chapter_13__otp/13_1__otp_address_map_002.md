---
source_pdf: rp2350-datasheet-8.pdf
repository: llm_database
chapter: Chapter 13. OTP
section: 13.1. OTP address map
pages: 1269-1270
type: technical_spec
generated_at: 2026-03-01T08:45:43.944040+00:00
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

RP2350 Datasheet

• 0x40138000, OTP_DATA_GUARDED_BASE: ECC guarded read alias. Successful reads return the same data as OTP_DATA_BASE.

Only the first 8 kB is populated.
• 0x40134000, OTP_DATA_RAW_BASE: raw read alias. A 32-bit read directly returns the 24-bit contents of a single row, with

zeroes in the eight MSBs, or returns all-ones on permission failure.
• 0x4013c000, OTP_DATA_RAW_GUARDED_BASE: raw, guarded read alias. Successful reads return the same data as

OTP_DATA_RAW_BASE.

Bit 14 of the address selects ECC (0) vs raw (1). Bit 15 of the address selects unguarded (0) vs guarded (1) access.

Guarded reads return the same data as unguarded reads, but perform additional hardware consistency checks and

return bus faults on permission failure. For more information, see Section 13.1.1.

IMPORTANT

The read data regions starting at 0x40130000 are accessible only when USR.DCTRL is set, otherwise all reads return a

bus error response. This bit is clear when the OTP is being programmed via the SBPI bridge.

Writing to the read data aliases is not a valid operation, and will always return a bus fault. The OTP is programmed by

the SBPI bridge, which is used internally by the bootrom otp_access API, Section 5.4.8.21.

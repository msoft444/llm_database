---
source_pdf: rp2350-datasheet-1.pdf
repository: llm_database
chapter: Chapter 13. OTP
section: 13.2. Background: OTP IP details
pages: 1270-1270
type: technical_spec
generated_at: 2026-02-28T17:22:56.644477+00:00
---

# 13.2. Background: OTP IP details

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
13.1.1. Guarded reads
Reads through the guarded aliases differ from unguarded reads in the following ways:
• Permission failures return bus faults rather than a bit pattern of all-ones.
• Uncorrectable ECC errors return a bus fault if detected.
• Guarded reads perform an additional hardware consistency check to detect power transients. If this check fails,
the read returns a bus fault.
These checks help to make the OTP fail-safe in contexts where deliberate fault injection is a possibility. For example,
the RP2350 bootrom uses guarded reads to check boot configuration flags.
The data returned from a successful guarded read is the same as the data returned by a successful read from the
corresponding unguarded alias.
IMPORTANT
Users relying on OTP data in a Secure context should always perform guarded reads, and it is strongly
recommended to use ECC. For rows where ECC is not possible, software should take care to ensure the consistency
of data across multiple overlapping reads.
13.2. Background: OTP IP details
The RP2350 OTP subsystem uses the Synopsys NVM OTP IP, which comes in 3 parts:
• Integrated Power Supply (IPS), including:
◦Charge Pump (for programming)
◦Regulator (for reading)
• OTP Macro (SHF, Fuse)
◦4096 × 24 (8 kB with ECC, 16-bit ECC write granularity)
• Access port (AP), providing:
RP2350 Datasheet
13.2. Background: OTP IP details
1269


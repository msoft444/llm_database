---
source_pdf: rp2350-datasheet-3.pdf
repository: llm_database
chapter: Chapter 13. OTP
section: 13.5.1. Lock progression
pages: 1275-1275
type: technical_spec
generated_at: 2026-03-01T02:30:35.833805+00:00
---

# 13.5.1. Lock progression

RP2350 Datasheet

debug disable flag.
• BOOT_ARCH (reset: 0 ): set the reset value of the ARCHSEL register (0 → Arm, 1 → RISC-V) if it has not been forced by

other critical flags.

◦Not critical, but hardware-read.
• GLITCH_DETECTOR_ENABLE (reset: 0): pass an enable signal to the glitch detectors so that they can be armed before any

software runs.
• GLITCH_DETECTOR_SENS(reset: 0): configure the initial sensitivity of the glitch detector circuits.

Critical flags are encoded with a three-of-eight vote across eight consecutive OTP rows. Each flag is redundantly

programmed to the same bit position in eight consecutive rows. Hardware considers the flag to be set if the bit reads as

1 in at least three of these eight rows. The flag is considered clear if no more than two bits are observed to be set.

NOTE

As of RP2350 A3 the ARM_DISABLE flag has no effect, removing a potential unlock path for debug on a secured

RP2350. Additionally, the combination of RISCV_DISABLE=1 and BOOT_ARCH=1 is decoded to an invalid state and the chip

will not boot.

JTAG disable is ignored only if the customer RMA flag (Section 13.7) is set.

For further discussion of the effects of the critical flags, see:

• Section 3.5.9.1 for the effects of the debug disable flags
• Section 3.9 for the effects of the Arm/RISC-V architecture select flags
• Section 10.9 for the effects of the glitch detector configuration flags
• Section 10.1.1 for discussion of the bootrom secure boot support enabled by the SECURE_BOOT_ENABLE flag

13.5. Page locks

The OTP protection hardware logically segments OTP into 64 pages (0 through 63), each 128 bytes in size, or

equivalently 64 OTP rows.

Each page has a set of lock registers which determine read and write access for that page from Secure and Non-secure

code. The lock registers are preloaded from OTP at reset, and can then be advanced (i.e. made less permissive) by

software. Lock registers themselves are always world-readable.

Pages 61 through 63 are not so neatly described by a single set of lock registers. These pages store lock initialisation

metadata. For more details, see Section 13.5.4. This section describes the more common case of a page protected by a

set of page locks.

13.5.1. Lock progression

Due to hardware constraints (Section 13.3.1), read and write restrictions are not orthogonal: it’s impossible to disallow

reads to an address without also disallowing writes. So, the progression of locking for a given page is:

0. Read/Write

1. Read-only

2. Inaccessible

Lock state only increases. This is enforced in two ways:

• Due to the nature of OTP and the choice of encoding, you cannot lower the OTP values preloaded to the lock

registers during boot.

13.5. Page locks
1274

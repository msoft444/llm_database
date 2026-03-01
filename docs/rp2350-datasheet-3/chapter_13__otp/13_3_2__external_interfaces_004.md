---
source_pdf: rp2350-datasheet-3.pdf
repository: llm_database
chapter: Chapter 13. OTP
section: 13.3.2. External interfaces
pages: 1272-1272
type: technical_spec
generated_at: 2026-03-01T02:30:35.833805+00:00
---

# 13.3.2. External interfaces

RP2350 Datasheet

• Read/Write
• Read-only
• Inaccessible

The full locking scheme is described in in Section 13.5, but to summarise:

• The lock state of each OTP page is read from OTP at boot time.
• There is a separate copy of the lock state for Secure/Non-secure accesses. The lock shim applies the Secure read

permissions to Secure reads, and the Non-secure read permissions to Non-secure reads. There is no such rule for

writes, because Non-secure code is not capable of accessing the programming hardware.
• The lock encoding in OTP storage is such that a page can always be locked down to a less permissive state (in the

order RW → RO → Inaccessible) but can never return to a more permissive state.
• Software can advance the state of each individual lock at runtime without programming OTP, and this lasts until

the OTP PSM is re-run.
• Software locks also obey the lock progression order (RW → RO → Inaccessible) and can not be regressed.

The full locking scheme is described in in Section 13.5.

13.3.2. External interfaces

The OTP integration has one upstream APB interface, which splits internally onto two separate interfaces. This

guarantees the hardware only serves a single upstream APB access at a time, with a single PPROT security level.

The first APB interface is the data interface (or data bridge) (OTPD). It has the following characteristics:

• Read-only
• Connects to the Synopsys device access port (DAP)
• Data interface reads always return 32 or 24 bits of valid data
• The data interface address is rounded down to a multiple of 32 bits, so that narrow reads return the correct byte

lanes
• There is an 8 kB window which supports 32-bit ECC reads

◦Each upstream bus read is split into two OTP accesses, each of which returns 16 bits of error-corrected data

from the OTP
• There is an 8 kB window which supports guarded 32-bit ECC reads, and returns a bus error if the guarding read

fails.

◦Functions the same as the ECC read window, but reads the Synopsys boot word before accessing the OTP

array, and return a bus error if the first read does not match the expected constant

◦Used to increase confidence in software OTP reads in the bootrom
• There is a 16 kB window which supports 24-bit raw reads

◦Each access returns a single raw 24-bit OTP row, bypassing error correction

◦Software must provide its own redundancy (e.g. triple majority vote)

◦Allows bit-mutable data structures, such as boot flags, or thermometer counters

The second APB interface is the command interface. This provides two main functions:

• Provides a bridge to the SBPI interface (Synopsys proprietary Serial and Byte-Parallel Interface bus)

◦SBPI connects to the Programmable Master Controller (PMC), with access to the DAP, DATAPATH, charge

pump (IPS), and fuse memory (SHF)

13.3. Background: OTP hardware architecture
1271

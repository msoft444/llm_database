---
source_pdf: rp2350-datasheet-9.pdf
repository: llm_database
chapter: Chapter 13. OTP
section: 13.4. Critical flags
pages: 1274-1275
type: technical_spec
generated_at: 2026-03-01T11:44:31.548839+00:00
---

# 13.4. Critical flags

13.4. Critical flags

Critical flags enable hardware security features which are fundamental to RP2350’s secure boot implementation. The

OTP power-up state machine reads critical flags very early in the system reset sequence, before any code runs on the

processors.

Most critical flags are in the main Boot Configuration page, page 1. These are listed under CRIT1 in the OTP data listing.

The exceptions are the Arm/RISC-V disable flags, which are in the Chip Info page, page 0. This page is made read-only

during factory programming, so users can not write to the CRIT0 flags.

Critical flags define 0 as the unprogrammed value, and 1 as the programmed value. On a blank device, all of the CRIT1

flags are 0. The reset value specified below is the value assigned to the internal logic net between the OTP reset being

applied and the OTP PSM completing. For example, the reset value of 1 for the debug disable flags implies that debug is

not accessible whilst the OTP PSM is running, but may be available afterward, depending on the value read from OTP

storage.

• ARM_DISABLE (reset: 0 ): Force the ARCHSEL register to RISC-V, at higher priority than RISC-V disable flag, secure boot

enable flag, or default boot architecture flag.
• RISCV_DISABLE (reset: 0): Force the ARCHSEL register to Arm, at higher priority than the default boot architecture flag.
• SECURE_BOOT_ENABLE (reset: 1 ): Enable boot signature checking in bootrom, disable factory JTAG, and force the

ARCHSEL register to Arm, at higher priority than the default boot architecture flag.
• SECURE_DEBUG_DISABLE (reset: 1 ): Disable factory JTAG, block Secure accesses from Mem-APs, and block halt

requests to Secure processors.

◦Prevents secure AP accesses by masking their ap_secure_en signals.

◦Prevents secure processor halting by masking the Cortex-M33’s SPIDEN and NSPIDEN signals.

◦Secure debug can be re-enabled by a Secure register in the OTP block.

◦Re-enable of Secure debug can be disabled by a Secure write-1-only lock register, also in the OTP block.
• DEBUG_DISABLE (reset: 1): Completely disable the Mem-APs, in addition to disabling everything disabled by the secure

13.4. Critical flags
1273

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

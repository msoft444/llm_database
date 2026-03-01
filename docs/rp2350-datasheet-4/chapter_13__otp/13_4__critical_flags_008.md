---
source_pdf: rp2350-datasheet-4.pdf
repository: llm_database
chapter: Chapter 13. OTP
section: 13.4. Critical flags
pages: 1274-1274
type: technical_spec
generated_at: 2026-03-01T03:27:49.792955+00:00
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

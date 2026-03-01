---
source_pdf: rp2350-datasheet-3.pdf
repository: llm_database
chapter: Chapter 13. OTP
section: 13.4. Critical flags
pages: 1274-1274
type: technical_spec
generated_at: 2026-03-01T02:30:35.833805+00:00
---

# 13.4. Critical flags

RP2350 Datasheet

PSM holds the rest of the system in reset until the OTP PSM completes, so that no software runs until the OTP’s

contents are known.

The OTP boot sequence runs from a local ring oscillator. This oscillator is dedicated to the OTP subsystem, and is

separate from the main system ROSC used by the processors at boot. The sequence is:

1. First, the PSM runs the Synopsys boot instruction. This has the following steps:

a. Wait for the power supply to return a 'good' value.

b. Read consistency check location until hardware sees the correct value for 16 successive reads. Consistency

checks use predefined words stored in mask ROM cells with similar analogue properties to OTP cells.

2. Read critical flags (non-ECC): each critical bit is redundant across 8 OTP rows, with three-of-eight vote for each

flag.

3. Read hardware access keys via ECC read interface.

4. Read valid bits for hardware access keys, including the debug keys (Section 3.5.9.2)

5. Initialise page lock registers from the lock page via raw read interface.

6. Assert rst_done signal to the system power-on state machine

7. The system reset sequence continues, starting with the system ROSC

RP2350 A3 adds correctness checks and robustness to the PSM. For more information about these additions, see

RP2350-E16.

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

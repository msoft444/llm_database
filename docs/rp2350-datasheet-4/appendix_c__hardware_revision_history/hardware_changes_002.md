---
source_pdf: rp2350-datasheet-4.pdf
repository: llm_database
chapter: Appendix C: Hardware revision history
section: Hardware changes
pages: 1355-1355
type: technical_spec
generated_at: 2026-03-01T03:27:49.792955+00:00
---

# Hardware changes

Hardware changes

Stepping A3 introduces the following hardware fixes and mitigations:

• Fix RP2350-E3: in QFN-60 package, GPIO_NSMASK controls wrong PADS registers. Hardware now remaps GPIO_NSMASK to

the correct PADS registers in the QFN60 package.
• Fix RP2350-E9: increased leakage current on Bank 0 GPIO when pad input is enabled. The pad circuit is modified

to eliminate the erroneous leakage path through the input buffer.
• Mitigate RP2350-E12: inadequate synchronisation of USB status signals. Signals used by the bootrom are now

valid across the full PVT range in the bootrom’s clock configuration. Other software must not rely on these

mitigations.
• Mitigate RP2350-E16: USB_OTP_VDD disruption can cause corrupt OTP row read data. The following changes apply:

◦Multiple changes to the OTP PSM and OTP read circuits to detect unreliable operation.

◦RISC-V debug is now disabled by CRIT1.SECURE_DEBUG_DISABLE, in addition to CRIT1.DEBUG_DISABLE. (On

A2, only the latter bit was used.)

◦CRIT0.ARM_DISABLE no longer disables the Arm processors.

◦Programming both CRIT0.ARM_DISABLE and CRIT0.RISCV_DISABLE is decoded as an illegal combination,

and the device won’t boot.

RP2350 A2
1354

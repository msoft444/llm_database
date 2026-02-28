---
source_pdf: rp2350-datasheet-2.pdf
repository: llm_database
chapter: Appendix C: Hardware revision history
section: Hardware changes
pages: 1355-1355
type: technical_spec
generated_at: 2026-02-28T17:43:10.557178+00:00
---

# Hardware changes

RP2350 Datasheet

Appendix C: Hardware revision
history

This appendix summarises the differences between RP2350 hardware revisions, referred to as steppings. To determine

the stepping of an unknown device, check the package markings, as described in Section 14.4. Software running on the

device can also read the CHIP_ID.REVISION register field, or call the rp2350_chip_version() SDK function.

In this appendix:

• The term fix refers to an issue that’s fully resolved.
• The term mitigate refers to an issue that’s either partially resolved or believed to be fully resolved but with an

unpredictable underlying cause, such as a fault injection vulnerability.
• The term update refers to any other difference between steppings.

This appendix offers a high-level overview; for detailed information, refer to individual errata entries in appendix E.

RP2350 A2

Stepping A2 is identified by a CHIP_ID.REVISION value of 0x2.

A2 is the first generally available version of RP2350, and the earliest stepping documented in this datasheet.

RP2350 A3

Stepping A3 is identified by a CHIP_ID.REVISION value of 0x3.

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

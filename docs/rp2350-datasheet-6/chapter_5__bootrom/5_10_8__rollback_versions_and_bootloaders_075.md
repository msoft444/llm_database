---
source_pdf: rp2350-datasheet-6.pdf
repository: llm_database
chapter: Chapter 5. Bootrom
section: 5.10.8. Rollback versions and bootloaders
pages: 441-442
type: technical_spec
generated_at: 2026-03-01T05:39:21.783275+00:00
---

# 5.10.8. Rollback versions and bootloaders

5.10.8. Rollback versions and bootloaders

WARNING

Ignoring the advice in this section could render your device unable to boot.

For bootloaders that need to chain into executable images with rollback versions on a secured RP2350, you must use

separate OTP rows for:

• The bootloader rollback version
• The chained executable image’s rollback version

Otherwise, bumping the version of the chained executable image renders the OTP bootloader and your device unable to

boot.

You must also make sure that both the bootloader and the executable image have non-zero rollback versions, as the

OTP flags relating to requiring rollback versions are global. Failure to do so will render your device unable to boot.

We recommend using the DEFAULT_BOOT_VERSION0 and DEFAULT_BOOT_VERSION1 rows for the binary’s rollback

version, and selecting some other unused rows in the OTP for the bootloader’s rollback version.

5.10. Example boot scenarios
440

RP2350 Datasheet

Chapter 6. Power

6.1. Power supplies

RP2350 requires five separate power supplies. However, in most applications, several of these can be combined and

connected to a single power source. Typical applications only require a single 3.3 V supply. See Figure 19.

The power supplies and a number of potential power supply schemes are described in the following sections. Detailed

power supply parameters are provided in Section 14.9.5.

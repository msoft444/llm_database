---
source_pdf: rp2350-datasheet-9.pdf
repository: llm_database
chapter: Chapter 5. Bootrom
section: 5.10.8. Rollback versions and bootloaders
pages: 441-441
type: technical_spec
generated_at: 2026-03-01T11:44:31.548839+00:00
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

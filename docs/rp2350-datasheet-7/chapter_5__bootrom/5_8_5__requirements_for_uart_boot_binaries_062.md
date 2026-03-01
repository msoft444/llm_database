---
source_pdf: rp2350-datasheet-7.pdf
repository: llm_database
chapter: Chapter 5. Bootrom
section: 5.8.5. Requirements for UART boot binaries
pages: 419-419
type: technical_spec
generated_at: 2026-03-01T06:07:53.318745+00:00
---

# 5.8.5. Requirements for UART boot binaries

RP2350 Datasheet

5.8.5. Requirements for UART boot binaries

A UART boot binary is a normal RAM binary. It must have a valid IMAGE_DEF in order for the boot path to recognise it as a

bootable binary. The search window for the IMAGE_DEF is the whole of SRAM, but it’s recommended to place it close to

the beginning, because the bootrom searches linearly forward for the beginning of the IMAGE_DEF.

The maximum size for a UART boot binary is the entirety of main SRAM: 520 kB, or 532 480 bytes.

UART boot only supports loading to the start of SRAM, so your binary must be linked to run at address 0x20000000.

Sparse loading is unsupported. Your program must load as a single flat binary image.

All security requirements relating to RAM image boot apply to UART boot too. If secure boot is enabled, your binary

must be signed. Likewise, if OTP anti-rollback versioning is in effect, your binary’s rollback version must be no lower

than the version number stored in OTP.

5.9. Metadata block details

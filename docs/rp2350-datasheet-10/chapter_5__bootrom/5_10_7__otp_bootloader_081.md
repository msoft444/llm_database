---
source_pdf: rp2350-datasheet-10.pdf
repository: llm_database
chapter: Chapter 5. Bootrom
section: 5.10.7. OTP bootloader
pages: 440-441
type: technical_spec
generated_at: 2026-03-01T12:20:14.494106+00:00
---

# 5.10.7. OTP bootloader

5.10.7. OTP bootloader

This is similar to the custom bootloader scenario, but it will be stored in the OTP and will run in SRAM.

One possible use case could place decryption code into OTP which decrypts an executable image from a flash partition

into RAM.

The entire bootloader will need to fit in the OTP rows from 0x0C0 to 0xF48 to avoid interfering with other reserved OTP

functionality, giving a maximum size of 7440 bytes (2 bytes per ECC row). If some boot keys and OTP keys are unused,

this region can extend slightly on either end.

5.10. Example boot scenarios
439

RP2350 Datasheet

The OTP bootloader itself should be stored in ECC format, starting from the row set in OTPBOOT_SRC with size set in

OTPBOOT_LEN. When booting, it will be loaded into the address specified in OTPBOOT_DST0 and OTPBOOT_DST1,

which must be in the main SRAM. The bootloader must fulfil the same criteria as a standard image: it must include an

IMAGE_DEF, which must be signed if secure boot is enabled.

Once the OTP bootloader has been written to OTP, and the OTPBOOT_SRC, OTPBOOT_LEN, OTPBOOT_DST0 and

OTPBOOT_DST1 set, OTP booting can be enabled by setting BOOT_FLAGS0.ENABLE_OTP_BOOT. If the OTP image fails

the bootrom’s launch checks, then, by default, boot continues along the normal flash boot path. You can prevent this by

setting BOOT_FLAGS0.DISABLE_FLASH_BOOT.

WARNING

Take extreme care when writing an OTP bootloader. Once the ECC rows are written, they cannot be modified.

---
source_pdf: rp2350-datasheet-4.pdf
repository: llm_database
chapter: Chapter 5. Bootrom
section: 5.10.7. OTP bootloader
pages: 440-440
type: technical_spec
generated_at: 2026-03-01T03:27:49.792955+00:00
---

# 5.10.7. OTP bootloader

![Page 440 figure](images/fig_p0440.png)

5.10.7. OTP bootloader

This is similar to the custom bootloader scenario, but it will be stored in the OTP and will run in SRAM.

One possible use case could place decryption code into OTP which decrypts an executable image from a flash partition

into RAM.

The entire bootloader will need to fit in the OTP rows from 0x0C0 to 0xF48 to avoid interfering with other reserved OTP

functionality, giving a maximum size of 7440 bytes (2 bytes per ECC row). If some boot keys and OTP keys are unused,

this region can extend slightly on either end.

5.10. Example boot scenarios
439

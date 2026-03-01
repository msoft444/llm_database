---
source_pdf: rp2350-datasheet-5.pdf
repository: llm_database
chapter: Chapter 5. Bootrom
section: 5.2.6. OTP boot
pages: 374-374
type: technical_spec
generated_at: 2026-03-01T04:13:03.328193+00:00
---

# 5.2.6. OTP boot

RP2350 Datasheet

5.2.6. OTP boot

If OTP boot is enabled, then code from OTP is executed in preference to code from flash. Note that the OTP code is free

to "chain" into an executable stored in flash.

Code from OTP is copied into SRAM at the specified location, then execution proceeds similarly to RAM Image Boot.

The SRAM with the data copied from OTP is searched for a valid (and correctly signed if necessary) IMAGE_DEF. If found, it

is booted; otherwise OTP boot falls through to Flash Boot (if enabled).

OTP boot could, for example, be used to execute some hidden decryption code to decode a flash image on startup. The

OTP boot code can hide itself (in OTP) even from Secure code, once it is done.

![Page 374 figure](images/fig_p0374.png)

---
source_pdf: rp2350-datasheet-10.pdf
repository: llm_database
chapter: Chapter 5. Bootrom
section: 5.2.9. Boot configuration (OTP)
pages: 376-376
type: technical_spec
generated_at: 2026-03-01T12:20:14.494106+00:00
---

# 5.2.9. Boot configuration (OTP)

5.2.9. Boot configuration (OTP)

User configuration stored in OTP can be found in Section 13.10, starting at CRIT1.

The main controls for the bootrom are stored in BOOT_FLAGS0 and BOOT_FLAGS1. These are both in page 1 of OTP,

which has the following default permissions on a blank device:

• Read-write for Secure (S)
• Read-write for bootloader (BL)
• Read-only for Non-secure (NS)

Boot key hashes are stored in page 2 of OTP, starting from BOOTKEY0_0. There is space for up to four boot key hashes

in this page. See Section 5.10.1 for an example of how keys can be installed.

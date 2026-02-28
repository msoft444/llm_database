---
source_pdf: rp2350-datasheet-2.pdf
repository: llm_database
chapter: Chapter 13. OTP
section: 13.6.1. Bit repair by polarity (BRP)
pages: 1278-1278
type: technical_spec
generated_at: 2026-02-28T17:43:10.557178+00:00
---

# 13.6.1. Bit repair by polarity (BRP)

RP2350 Datasheet

• Page 0, which contains chip information, is read-only for all accesses.
• Pages 1 and 2, which contain boot config and boot key fingerprints, are read-only for Non-secure access, read-

write for Secure access, and read-write for bootloader access.
• Page 62, which contains only the page 62 lock word, is read-only for Non-secure access, read-write for Secure

access, and read-write for bootloader access (as a partial workaround for RP2350-E28).
• Page 63, which contains the RMA flag, is read-only for Non-secure and bootloader access, and read-write for

Secure access.

This minimal set of default permissions on blank devices avoids certain classes of security model violation, like Non-

secure code being able to brick the chip by overwriting the boot key fingerprints with invalid data. In this context, the

term blank device refers to a device that has gone through manufacturing test programming, but has not had any other

OTP bits programmed by the user.

You can add additional soft or hard locks to these default permissions, with the exception of page 0. Page 0 cannot be

hard-locked, since the secure read-only permission prevents a user from altering its lock word.

Lock words 2 through 61, covering all pages with user-defined contents, are left unprogrammed. On a blank device,

these pages are fully accessible from all domains. Before launching any Non-secure application, you should apply at

least a soft read-only lock to all pages that are not explicitly allocated for Non-secure use. To do this, write to

SW_LOCK2 through SW_LOCK61. For devices that you don’t expect to RMA, such as those that have passed board-level

manufacturing tests, you should lock secure writes to the RMA flag.

13.6. Error Correction Code (ECC)

ECC-protected rows store data in the following structure, accessible through a raw alias:

• Bits 23:22: bit repair by polarity (BRP) flag
• Bits 21:16: modified Hamming ECC code
• Bits 15:0 (the 16 LSBs): data

RP2350 stores the following error correction data in the 8 MSBs of each 24-bit row:

• a 6-bit modified Hamming code ECC, providing single-error-correct and double-error-detect capabilities
• 2 bits of bit repair by polarity (BRP), which supports inverting the entire row at programming time to repair a single

set bit that should be clear

Writes first encode ECC, then BRP. Reads first decode BRP, then ECC. When reading through an ECC data alias (Section

13.1), hardware performs correction transparently. ECC programming operations (writes) automatically generate ECC

bits when you use the bootrom otp_access API (Section 5.4.8.21).

ECC is not suitable for data that mutates one bit at a time, since the ECC value is derived from the entire 16-bit data

value. When storing data without ECC, use another form of redundancy, such as 3-way majority vote.

13.6.1. Bit repair by polarity (BRP)

Bit repair by polarity (BRP) compensates for a single bit present at time of programming.

When programming a row, hardware or software first calculates a 24-bit target value consisting of:

• Two zeroes in bits 23:22
• 6-bit Hamming ECC in bits 21:16
• 16-bit data value in bits 15:0

Before programming, an OTP row should contain all zeros. However, sometimes OTP rows contain a single bit that is

already set to 1, either due to manufacturing flaws or previous programming. If a bit is already set (1) in an OTP row

13.6. Error Correction Code (ECC)
1277

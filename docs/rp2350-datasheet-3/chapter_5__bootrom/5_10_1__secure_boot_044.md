---
source_pdf: rp2350-datasheet-3.pdf
repository: llm_database
chapter: Chapter 5. Bootrom
section: 5.10.1. Secure boot
pages: 430-430
type: technical_spec
generated_at: 2026-03-01T02:30:35.833805+00:00
---

# 5.10.1. Secure boot

![Page 430 figure](images/fig_p0430.png)

RP2350 Datasheet

| Word | LE Value | Bytes | Description |
| --- | --- | --- | --- |
| 0 | 0xffffded3 | 4 | PICOBIN BLOCK MARKER START _ _ _ |
| 1 | 0x11010142 | 1 | 0x42(item_type == PICOBIN_BLOCK_ITEM_1BS_IMAGE_TYPE) |
|  |  | 1 | 0x01 (Item is 1 word in size) |
|  |  | 2 | 0x1101 (PICOBIN IMAGE TYPE IMAGE TYPE AS BITS(EXE) | _ _ _ _ _ _ PICOBIN IMAGE TYPE EXE CPU AS BITS(RISCV) | _ _ _ _ _ _ PICOBIN IMAGE TYPE EXE CHIP AS BITS(RP23500)) _ _ _ _ _ _ |
| 2 | 0x000001ff | 1 | 0xff(size_type == 1, item_type_ == PICOBIN BLOCK ITEM 2BS LAST) _ _ _ _ |
|  |  | 2 | 0x0001 (size) |
|  |  | 1 | 0x00 (pad) |
| 3 | 0x00000000 | 4 | Relative pointer to next block in block loop - 0x00000000 means link to self (a loop containing just this block) |
| 4 | 0xab123579 | 4 | PICOBIN BLOCK MARKER END _ _ _ |

The LE Value column indicates a 32-bit little-endian value that should appear verbatim in your program image.

Since the above block does not specify an explicit entry point, the bootrom will enter the binary at its lowest address,

which 
is 
the 
default 
behaviour 
on 
RISC-V. 
This 
default 
entry 
point 
can 
be 
overridden 
by 
a

PICOBIN_BLOCK_ITEM_1BS_ENTRY_POINT item. Note that PICOBIN_BLOCK_ITEM_1BS_VECTOR_TABLE is not valid on RISC-V, as unlike

Cortex-M the RISC-V vector table does not define the program entry point.

5.10. Example boot scenarios

This section describes the setup and configuration steps for various different boot scenarios.

5.10.1. Secure boot

To enable secure boot on RP2350, you must:

1. Set the SHA-256 hashes of the boot keys you will be using in BOOTKEY0_0 onwards

2. Set bits in BOOT_FLAGS1.KEY_VALID for the keys you will be using

3. Optionally set bits in BOOT_FLAGS1.KEY_INVALID for all unused keys — this is recommended to prevent a

malicious actor installing their own boot keys at a later date

4. Set CRIT1.SECURE_BOOT_ENABLE to turn on secure boot.

NOTE

These steps are the minimum for enabling secure boot support in the bootrom. See Section 10.5 for additional steps

you must take to fully secure your device, such as disabling hardware debug.

All of the above can be achieved with picotool. For example, when signing using picotool seal you can add an OTP JSON

output file, to which it will add the relevant OTP field values to enable secure boot (BOOTKEY0_0,

BOOT_FLAGS1.KEY_VALID and CRIT1.SECURE_BOOT_ENABLE):

5.10. Example boot scenarios
429

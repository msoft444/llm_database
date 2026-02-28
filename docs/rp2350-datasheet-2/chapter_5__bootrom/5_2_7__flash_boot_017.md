---
source_pdf: rp2350-datasheet-2.pdf
repository: llm_database
chapter: Chapter 5. Bootrom
section: 5.2.7. Flash boot
pages: 374-374
type: technical_spec
generated_at: 2026-02-28T17:43:10.557178+00:00
---

# 5.2.7. Flash boot

![Page 374 figure](images/fig_p0374.png)

RP2350 Datasheet

5.2.6. OTP boot

If OTP boot is enabled, then code from OTP is executed in preference to code from flash. Note that the OTP code is free

to "chain" into an executable stored in flash.

Code from OTP is copied into SRAM at the specified location, then execution proceeds similarly to RAM Image Boot.

The SRAM with the data copied from OTP is searched for a valid (and correctly signed if necessary) IMAGE_DEF. If found, it

is booted; otherwise OTP boot falls through to Flash Boot (if enabled).

OTP boot could, for example, be used to execute some hidden decryption code to decode a flash image on startup. The

OTP boot code can hide itself (in OTP) even from Secure code, once it is done.

5.2.7. Flash boot

The bootrom scans flash up to 16 times until it finds a valid IMAGE_DEF or PARTITION_TABLE. At this point, the flash settings

are considered valid, and the flash boot proceeds if a valid bootable IMAGE_DEF is found with these settings. It uses the

following combinations of flash read instruction and SCK divisor for the 16 attempts:

| Mode | Clock Divisor |
| --- | --- |
| EBh quad | 3 |
| BBh dual | 3 |
| 0Bh serial | 3 |
| 03h serial | 3 |
| EBh quad | 6 |
| BBh dual | 6 |
| 0Bh serial | 6 |
| 03h serial | 6 |
| EBh quad | 12 |
| BBh dual | 12 |
| 0Bh serial | 12 |
| 03h serial | 12 |
| EBh quad | 24 |
| BBh dual | 24 |
| 0Bh serial | 24 |
| 03h serial | 24 |

Table 452. QSPI read

modes supported by

the bootrom, in the

order it attempts

them.

QSPI does not provide a reliable method to detect whether a device is attached. However, this is not much of an issue

for boot purposes: either there is a device with valid and bootable contents, or there are no such contents (either due to

lack of a connected device, invalid device contents, or failure to communicate in the current QSPI mode).

When there is no device (or no recognisable contents), the bootrom tries all 16 modes in Table 452 before finally giving

up. The size of the initial search region is limited to 4 kB to minimise the time spent scanning flash before falling

through to USB or UART boot. This same 4 kB limit also applies to search within a flash partition, which allows the

bootrom to reliably sever the contained image’s block loop with a single 4 kB sector erase at the start of a partition,

such as on a version downgrade.

There are three main ways that the bootrom locates flash images:

5.2. Processor-controlled boot sequence
373

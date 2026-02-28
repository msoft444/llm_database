---
source_pdf: rp2350-datasheet-1.pdf
repository: llm_database
chapter: Chapter 5. Bootrom
section: 5.1.20. Automatic architecture switching
pages: 365-365
type: technical_spec
generated_at: 2026-02-28T17:22:56.644477+00:00
---

# 5.1.20. Automatic architecture switching

As an example, if the booted partition was 6 MB big, starting 1 MB into flash, the registers would be set up as follows:
Name / Memory Start
Flash Start
Size
ATRANS0 / 0x10000000
0x10100000
4 MB
ATRANS1 / 0x10400000
0x10500000
2 MB
ATRANS2 / 0x10800000
-
0
ATRANS3 / 0x10c00000
-
0
This maps the physical flash range from 0x10100000-0x10700000 to flash addresses 0x10000000-0x10600000, ensuring the
partition appears at the start of flash at runtime.
In this case, since ATRANS2 and ATRANS3 aren’t needed to map the partition, they could be used by the application to
map another part of flash.
Mapping the start of the partition to a runtime address of 0x10000000 is the default behaviour, but you may choose a
different address, with some restrictions. The bootrom allows for runtime address values of 0x10000000, 0x10400000,
0x10800000, and 0x10c00000 for the beginning of the mapped regions, with the choice specified in the IMAGE_DEF. You must
link your binary to run at the correct, higher base address. This is useful, for example, when an application runs from a
high flash address and remains mapped there while launching a second application at address 0x10000000. You might
use this setup when a Secure image provides services to a Non-secure client image.
This custom address translation is enabled by a negative ROLLING_WINDOW_DELTA value (see Section 5.9.3.5). The above
four runtime addresses translate to a ROLLING_WINDOW_DELTA of 0, -0x400000, -0x800000, or -0xc00000, which are the only
supported non-positive values. The delta indicates how many bytes into the partition the runtime address 0x10000000
appears. A negative delta value indicates that address 0x10000000 appears before the start of the partition; the partition
starts that many bytes higher than 0x10000000.
Positive values are also useful, for example, when prepending data to an already linked image as a post-processing
step. Positive deltas must be multiples of 4 kB. For example, a ROLLING_WINDOW_DELTA of 0x1000 will set up address
translation so that the image data starting at offset 0x1000 within the partition is mapped to 0x10000000 at runtime,
omitting the first 4 kB of the image from the mapped region. The first 4 kB is inaccessible except through the
untranslated XIP window, which defaults to Secure access only.
NOTE
Because address translation within the 0x100000000 → 0x11000000 and 0x11000000 → 0x12000000 windows is independent,
it is only possible to boot from partitions which are entirely contained within the first 16 MB of flash.
This address translation is performed by hardware in the QMI. For more information, see Section 12.14.4.
5.1.20. Automatic architecture switching
If the bootrom encounters a valid and correctly signed IMAGE_DEF for the non-current architecture (RISC-V when booted in
Arm mode, or Arm when booted in RISC-V), it performs an automatic architecture switch. The bootrom initiates a reboot
into the correct architecture for the binary it discovered, which then boots successfully on the second attempt.
Information passed in watchdog scratch registers (such as a RAM image boot type) is retained, so that the second boot
makes the same decisions as the first, and arrives at the same preferred image to boot.
This happens only when:
• The architecture to be switched to is available according to OTP critical flags
• The architecture switch feature is not disabled by the BOOT_FLAGS0.DISABLE_AUTO_SWITCH_ARCH flag
• The bootrom found no valid binary for the current architecture before finding one for the other architecture
RP2350 Datasheet
5.1. Bootrom concepts
364


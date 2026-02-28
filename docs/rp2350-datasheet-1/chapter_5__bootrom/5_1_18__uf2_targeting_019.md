---
source_pdf: rp2350-datasheet-1.pdf
repository: llm_database
chapter: Chapter 5. Bootrom
section: 5.1.18. UF2 targeting
pages: 363-363
type: technical_spec
generated_at: 2026-02-28T17:22:56.644477+00:00
---

# 5.1.18. UF2 targeting

NOTE
Flash update and version downgrade have no effect when using a single slot, or standalone (non A/B) partitions.
5.1.17. Try before you buy
Try before you buy (abbreviated TBYB) is an IMAGE_DEF-only feature that allows for a completely safe cycle of version
upgrade:
1. An executable image is running from say partition B.
2. A new image is downloaded into partition A.
3. On download completion, a FLASH_UPDATE reboot is performed for the newly updated partition A.
4. The bootrom will preferentially try to boot partition A (due to the flash update). Note that a non TBYB image will
always be chosen over a TBYB image in A/B partitions during a normal non-FLASH_UPDATE boot.
◦If the new image fails validation/signature then the old image in partition B will be used on subsequent (non-
FLASH_UPDATE) boots, recovering from the failed upgrade.
5. If the new image is valid (and correctly signed if necessary), it is entered under a watchdog timer, and has 16.7
seconds to mark itself OK via the explicit_buy() function.
◦If the image calls back, the first 4 kB sector of the other partition (containing image B) is erased, and the
TBYB flag of the current image is cleared, so that A becomes the preferred partition for subsequent boots.
◦If the image does not call back within the allotted time, then the system reboots, and will continue to boot
partition B (containing the original image) as partition A is still marked as TBYB image.
The erase of the first sector of the opposite partition in the A/B pair severs its image’s block loop, rendering it
unbootable. This ensures the tentative image booted under TBYB becomes the preferred boot image going forward,
even if the opposite image had a higher version.
The watchdog timeout is fixed at 16.7 seconds (24-bit count on a 1-microsecond timebase). This can be shortened after
entering the target image, for example if it only needs a few hundred milliseconds for its self-test routine. It can also be
extended by reloading the watchdog counter, at the risk of getting stuck in the tentative image if it fails in a way that
repeatedly reloads the watchdog.
5.1.18. UF2 targeting
Section 5.5 describes the USB Mass Storage drive, and the ability to download UF2 files to that drive to store and/or
execute code/data on the RP2350.
Since RP2350 supports multiple processor architectures, and partition tables with multiple partitions, some information
on the device must be used to determine what to do with a flash-addressed UF2. Depending on the context, the flash
addresses in the UF2 may be absolute flash storage addresses (as was always the case on RP2040), or runtime
addresses of code and data within a flash partition. UF2 targeting refers to the rules the bootrom applies to interpret
flash addresses in a UF2 file.
UF2 supports a 32-bit family ID embedded in the file. This enables the device to recognise firmware that targets it
specifically, as opposed to firmware intended for some other device. The RP2350 bootrom recognises some standard
UF2 family IDs (rp2040, rp2350-arm-s, rp2350-arm-ns, rp2350-riscv, data and absolute) defined in Table 455. You may define
your own family IDs in the partition table for more refined targeting.
The UF2 family ID is used as follows:
1. A UF2 with the absolute family ID is downloaded without regard to partition boundaries. A partition table (if present)
or OTP configuration define whether absolute family ID downloads are allowed. The default factory settings do
allow for absolute family ID downloads.
RP2350 Datasheet
5.1. Bootrom concepts
362


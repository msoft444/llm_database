---
source_pdf: rp2350-datasheet-4.pdf
repository: llm_database
chapter: Chapter 5. Bootrom
section: 5.1.18. UF2 targeting
pages: 363-363
type: technical_spec
generated_at: 2026-03-01T03:27:49.792955+00:00
---

# 5.1.18. UF2 targeting

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

5.1. Bootrom concepts
362

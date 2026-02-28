---
source_pdf: rp2350-datasheet-1.pdf
repository: llm_database
chapter: Chapter 5. Bootrom
section: 5.6.1. Identifying the device
pages: 404-404
type: technical_spec
generated_at: 2026-02-28T17:22:56.644477+00:00
---

# 5.6.1. Identifying the device

used to determine the status of the last download in this case (see also GET_INFO - UF2_STATUS).
5.5.3.1. A/B partitions and ownership
Each of the above passes refers to finding an A partition. Any partition that isn’t a B partition is an A partition; an unpaired
partition is classed as an A partition.
If the found A partition doesn’t have a B partition paired with it, then the A partition is the UF2 target partition.
If however, the A partition has a B partition, then a further choice must be made as to which of the A/B partitions should
be targeted.
1. If the A partition is unowned, then the partition choice is made based on any current valid IMAGE_DEF in those
partitions. The valid partition with the higher version number isn’t chosen; in the case of executable IMAGE_DEFs, this
is the opposite of what would happen during boot; this makes sense as you want to drop the UF2 on the partition
which isn’t currently booting.
2. If the A partition is marked owned, then the contents of the A partition and B partition are assumed not to contain
IMAGE_DEFs which can be used to make a version based choice. Therefore, the owner of the A partition (Aowner) and its
B partition (Bowner) are used to make the choice
It is however dependent on the use case whether you would want a UF2 that is destined for partition A / partition B
to go into partition A when partition Aowner has an IMAGE_DEF with the higher version (would boot if the IMAGE_DEF was
executable) or when Bowner has an IMAGE_DEF with the higher version. By default, the bootrom picks partition A when
partition 
Aowner has the higher versioned 
IMAGE_DEF, however this can be changed by setting the
UF2_DOWNLOAD_AB_NON_BOOTABLE_OWNER_AFFINITY flag in partition A.
5.5.3.2. Multiple UF2 families
It is possible to include sectors targeting different family IDs in the same UF2 file. The intention in the UF2 specification
is to allow one file to be shipped for multiple different devices, but the expectation is that each device only accepts one
UF2 family ID.
Similarly on RP2350, it is only supported to download a UF2 file containing multiple family IDs if only one of those family
IDs is acceptable for download to the device according to the above rules.
5.6. USB PICOBOOT interface
The PICOBOOT interface is a low level USB protocol for interacting with the RP2350 while it is in BOOTSEL mode. This
interface may be used concurrently with the USB Mass Storage Interface.
It provides for flexible reading from and writing to RAM or flash, rebooting, executing code on the device and a handful
of other management functions.
Constants and structures related to the interface can be found in the SDK header picoboot.h in the SDK
5.6.1. Identifying the device
A RP2350 device can recognised by the Vendor ID and Product ID in its device descriptor (shown in Table 456), unless
different values have been set in OTP (see Section 5.7)
Table 456. RP2350
Boot Device
Descriptor
Field
Value
bLength
18
bDescriptorType
1
RP2350 Datasheet
5.6. USB PICOBOOT interface
403


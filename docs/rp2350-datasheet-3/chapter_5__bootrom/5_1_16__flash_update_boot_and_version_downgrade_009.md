---
source_pdf: rp2350-datasheet-3.pdf
repository: llm_database
chapter: Chapter 5. Bootrom
section: 5.1.16. Flash update boot and version downgrade
pages: 362-362
type: technical_spec
generated_at: 2026-03-01T02:30:35.833805+00:00
---

# 5.1.16. Flash update boot and version downgrade

RP2350 Datasheet

Slot 0 starts at the beginning of flash, and has a size of n × 4 kB sectors. Slot 1 has the same size and follows

immediately after slot 0. The value of n defaults to 1. Both slots are 4 kB in size, but you can override this value by

specifying 
a 
value 
in 
FLASH_PARTITION_SLOT_SIZE 
and 
then 
setting

BOOT_FLAGS0.OVERRIDE_FLASH_PARTITION_SLOT_SIZE.

Similarly to how a choice can be made between IMAGE_DEFs in A/B partitions, a choice can be made between A/B

PARTITION_TABLEs via the two boot slots. This allows for versioning partition tables, targeted drag and drop of UF2s

(Section 5.1.18) containing partition tables, etc. similar to the process used for images.

Slot 1 is only of use when potentially using partition tables. In the simple case of an IMAGE_DEF and no PARTITION_TABLE

found in a block loop starting in slot 0, that image likely actually overlays the space where slot 1 would be, but in any

case, slot 1 is ignored since there is no PARTITION_TABLE.

If slot 0 contains a PARTITION_TABLE or does not contain an IMAGE_DEF (including nothing/garbage in slot 0), slot 1 can be

considered. As an optimisation, in the former case, the scanning of slot 1 can be prevented by setting the singleton flag

in the PARTITION_TABLE.

NOTE

When IMAGE_DEFs are also present in the slots, the PARTITION_TABLE's VERSION item determines which of slot 0 and slot 1

to use. The IMAGE_DEF metadata is ignored for the purpose of version comparison.

5.1.16. Flash update boot and version downgrade

Normally the choice of slot 0 versus slot 1, and partition A versus partition B, is made based on the version of the valid

PARTITION_TABLE or IMAGE_DEF in those slots or partitions respectively. The greater of the two versions wins.

It is however perfectly valid to downgrade to a lower-versioned IMAGE_DEF when using A/B partitions, provided this does

not violate anti-rollback rules on a secured RP2350.

Downloading the new image (and its IMAGE_DEF) into the non-currently-booting partition and doing a normal reboot will

not work in this case, as the newly downloaded image has a lower version.

For this purpose, you can enable a flash update boot boot by passing the FLASH_UPDATE boot type constant flag through

the watchdog scratch registers and a pointer to the start of the region of flash that has just been updated.

The bootrom automatically performs a flash update boot after programming a flash UF2 written to the USB Mass

Storage drive. You can also invoke a flash update boot programmatically via the reboot() API (see Section 5.4.8.24).

The flash address range passed through the reboot parameters is treated specially during a flash update boot. A

PARTITION_TABLE in a slot, or IMAGE_DEF in a partition, will be chosen for boot irrespective of version, if the start of the region

is the start of the respective slot or partition.

In order for the downgrade to persist, the first sector of the previously booting slot or partition must be erased so that

the newly installed PARTITION_TABLE or IMAGE_DEF will continue to be chosen on subsequent boots. This erase is performed

as follows during a FLASH_UPDATE boot.

1. When a PARTITION_TABLE is valid (and correctly signed if necessary) and its slot is chosen for boot, the first sector of

the other slot is erased.

2. When a valid (and correctly signed if necessary) IMAGE_DEF is launched, the first sector of the other image is erased.

3. On explicit request by the image, after it is launched, the first sector of the other image is erased. This is an

alternative to the standard behaviour in the previous bullet, and is selected by a special "Try Before You Buy" flag in

the IMAGE_DEF. For more information about this feature, see Section 5.1.17.

5.1. Bootrom concepts
361

---
source_pdf: rp2350-datasheet-10.pdf
repository: llm_database
chapter: Chapter 5. Bootrom
section: 5.10.4. A/B booting
pages: 434-436
type: technical_spec
generated_at: 2026-03-01T12:20:14.494106+00:00
---

# 5.10.4. A/B booting

5.10.4. A/B booting

This is a common boot scenario, to be able to update the software without overwriting it. A simple partition layout

would be:

5.10. Example boot scenarios
433

RP2350 Datasheet

```c
Partition 0
  Accepts UF2 Families: rp2350-arm-s, rp2350-riscv
Partition 1
  Accepts UF2 Families: rp2350-arm-s, rp2350-riscv
  Link Type: "A"
  Link Value: 0
```

This is a partition table with 2 partitions, where partition 0 is the A partition of partition 1 (which is thus the B partition).

NOTE

To avoid confusion, it is a recommended best practice to have the same permissions for both partitions, and for

both partitions to accept the same UF2 families. The bootrom will only look at the UF2 families from the A partition

when deciding if a given A/B pair accepts a particular family, and will not allow download to partition A even if it is

writable if partition B isn’t.

When dragging a UF2 to the device, it will target whichever partition is not currently booting. The bootrom will then

perform a FLASH_UPDATE boot into the new binary (see Section 5.1.16)

NOTE

When starting with blank A/B partitions, the first download actually goes into partition B.

To create the partition table above with picotool partition create, the following json could be used:

```c
{
  "version": [1, 0],
  "unpartitioned": {
    "families": ["absolute"],
    "permissions": {
      "secure": "rw",
      "nonsecure": "rw",
      "bootloader": "rw"
    }
  },
  "partitions": [
    {
      "name": "Example A",
      "id": 0,
      "size": "2044K",
      "families": ["rp2350-arm-s", "rp2350-riscv"],
      "permissions": {
        "secure": "rw",
        "nonsecure": "rw",
        "bootloader": "rw"
      }
    },
    {
      "name": "Example B",
      "id": 1,
      "size": "2044K",
      "families": ["rp2350-arm-s", "rp2350-riscv"],
      "permissions": {
        "secure": "rw",
        "nonsecure": "rw",
        "bootloader": "rw"
      },
      "link": ["a", 0]
```

5.10. Example boot scenarios
434

RP2350 Datasheet

```c
    }
  ]
}
```

This can then be installed onto the device using picotool load, or UF2 drag and drop if you output the partition table as a

UF2 file.

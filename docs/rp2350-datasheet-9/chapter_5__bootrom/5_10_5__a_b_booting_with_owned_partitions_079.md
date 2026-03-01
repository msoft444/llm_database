---
source_pdf: rp2350-datasheet-9.pdf
repository: llm_database
chapter: Chapter 5. Bootrom
section: 5.10.5. A/B booting with owned partitions
pages: 436-438
type: technical_spec
generated_at: 2026-03-01T11:44:31.548839+00:00
---

# 5.10.5. A/B booting with owned partitions

5.10.5. A/B booting with owned partitions

The concept of owned partitions applies when:

• you require separate data partitions (which generally won’t contain a block loop), but
• you would like these to be associated with a specific boot partition in an A/B pair

An example partition table for this scenario would be:

```c
Partition 0
  Accepts Families: rp2350-arm-s, rp2350-riscv
Partition 1
  Accepts Families: rp2350-arm-s, rp2350-riscv
  Link Type: "A"
  Link Value: 0
Partition 2
  Accepts Families: data
  Link Type: "Owner"
  Link Value: 0
  ignored_during_arm_boot: true
  ignored_during_riscv_boot: true
Partition 3
  Accepts Families: data
  Link Type: "A"
  Link Value: 2
  ignored_during_arm_boot: true
  ignored_during_riscv_boot: true
```

This is a partition table with 4 partitions. As before, partition 0 is the A partition of partition 1 (which is thus a B

partition). Additionally, partition 2 is the A partition of partition 3 (which is thus a B partition). Finally, partition 0 is the

"owner" partition of partition 2.

As a result partitions 2 and 3 "belong to" partitions 0 and 1.

When downloading a UF2 into an owned partition, the bootloader will select which partition out of 2/3 it goes to target

based on which partition out of 0/1 is currently booting. For example, if partition 1 is currently booting (due to having a

higher version than partition 0), then any UF2 downloads with the data family ID will target partition 3.


TIP

There is a flag in each partition that you can use to swap the "affinity", for example, to have the data family ID target

partition 2 instead of partition 3 in the scenario above.

5.10. Example boot scenarios
435

RP2350 Datasheet

NOTE

Only the get_uf2_target_partition() bootrom function considers owner partitions. The pick_ab_partition() function

always pick solely based on the A/B partition it is passed, in other words if passed partition 2, it would not look at

partitions 0 and 1.

To create this partition table using picotool partition create, the following JSON could be used:

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
      "size": 128k,
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
      "size": 128k,
      "families": ["rp2350-arm-s", "rp2350-riscv"],
      "permissions": {
        "secure": "rw",
        "nonsecure": "rw",
        "bootloader": "rw"
      },
      "link": ["a", 0]
    },
    {
      "name": "Example a",
      "id": 2,
      "size": 20k,
      "families": ["data"],
      "permissions": {
        "secure": "rw",
        "nonsecure": "rw",
        "bootloader": "rw"
      },
      "link": ["owner", 0],
      "ignored_during_arm_boot": true,
      "ignored_during_riscv_boot": true
    },
    {
      "name": "Example b",
      "id": 3,
      "size": 20k,
      "families": ["data"],
```

5.10. Example boot scenarios
436

RP2350 Datasheet

```c
      "permissions": {
        "secure": "rw",
        "nonsecure": "rw",
        "bootloader": "rw"
      },
      "link": ["a", 2],
      "ignored_during_arm_boot": true,
      "ignored_during_riscv_boot": true
    }
  ]
}
```

This can then be installed onto the device using picotool load, or UF2 drag and drop if you output the partition table as a

UF2 file.

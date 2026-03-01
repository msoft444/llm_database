---
source_pdf: rp2350-datasheet-3.pdf
repository: llm_database
chapter: Chapter 5. Bootrom
section: 5.10.4. A/B booting
pages: 434-435
type: technical_spec
generated_at: 2026-03-01T02:30:35.833805+00:00
---

# 5.10.4. A/B booting

![Page 434 figure](images/fig_p0434.png)

RP2350 Datasheet

5.10.3. Packaged binaries

A packaged binary is an SRAM/XIP RAM-only binary that has been post-processed for storage in flash. To create a

packaged SRAM binary, you can take a binary compiled to run in SRAM (no_flash binary in the SDK) and add a relative

LOAD_MAP Item into the IMAGE_DEF block, with the runtime address(es) in SRAM. The subsequent binary can then be run

normally from RAM, or stored in flash to be loaded into RAM by the bootrom. This LOAD_MAP item will be added to all

binaries when using picotool seal.

To package binaries in the SDK, add the following to your CMakeLists.txt file. This will target the UF2 file to the start of

flash when dragged and dropped, and will invoke picotool seal to add an appropriate LOAD_MAP.

pico_package_uf2_output(target_name 0x10000000)

Alternatively, you can use an absolute LOAD_MAP, with the storage_address in flash and the runtime_address in SRAM.

However, these binaries can only run after storing in flash and can’t be booted directly in SRAM for debugging.

For example, if you have a binary compiled to run at 0x20000000 of length 0x8000, and a metadata block at the end of the

binary containing the LOAD_MAP as the second Item (after the IMAGE_DEF, which means the LOAD_MAP is 8 bytes into the

block), then the relative LOAD_MAP would be:

Word
Bytes
Value

0
1
0x06 (size_flag == 0, item_type == PICOBIN_BLOCK_ITEM_LOAD_MAP)

2
0x04 (Block size in words)

1
0x01 (absolute == 0, num_entries == 1)

1-3
Load Map Entry 0

4
-0x8008 = 0xFFFF7FF8 (storage_start_address_rel)

4
0x20000000 (runtime_start_address)

4
0x8000 (size in bytes)

The absolute LOAD_MAP would be:

Word
Bytes
Value

0
1
0x06 (size_flag == 0, item_type == PICOBIN_BLOCK_ITEM_LOAD_MAP)

2
0x04 (Block size in words)

1
0x81 (absolute == 1, num_entries == 1)

1-3
Load Map Entry 0

4
0x10000000 (storage_start_address)

4
0x20000000 (runtime_start_address)

4
0x20008000 (runtime_end_address)

5.10.4. A/B booting

This is a common boot scenario, to be able to update the software without overwriting it. A simple partition layout

would be:

5.10. Example boot scenarios
433

![Page 435 figure](images/fig_p0435.png)

RP2350 Datasheet

Partition 0

  Accepts UF2 Families: rp2350-arm-s, rp2350-riscv

Partition 1

  Accepts UF2 Families: rp2350-arm-s, rp2350-riscv

  Link Type: "A"

  Link Value: 0

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

5.10. Example boot scenarios
434

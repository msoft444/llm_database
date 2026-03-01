---
source_pdf: rp2350-datasheet-5.pdf
repository: llm_database
chapter: Chapter 5. Bootrom
section: 5.10.3. Packaged binaries
pages: 434-434
type: technical_spec
generated_at: 2026-03-01T04:13:03.328193+00:00
---

# 5.10.3. Packaged binaries

RP2350 Datasheet

5.10.3. Packaged binaries

A packaged binary is an SRAM/XIP RAM-only binary that has been post-processed for storage in flash. To create a

packaged SRAM binary, you can take a binary compiled to run in SRAM (no_flash binary in the SDK) and add a relative

LOAD_MAP Item into the IMAGE_DEF block, with the runtime address(es) in SRAM. The subsequent binary can then be run

normally from RAM, or stored in flash to be loaded into RAM by the bootrom. This LOAD_MAP item will be added to all

binaries when using picotool seal.

To package binaries in the SDK, add the following to your CMakeLists.txt file. This will target the UF2 file to the start of

flash when dragged and dropped, and will invoke picotool seal to add an appropriate LOAD_MAP.

![Page 434 figure](images/fig_p0434.png)

pico_package_uf2_output(target_name 0x10000000)

Alternatively, you can use an absolute LOAD_MAP, with the storage_address in flash and the runtime_address in SRAM.

However, these binaries can only run after storing in flash and can’t be booted directly in SRAM for debugging.

For example, if you have a binary compiled to run at 0x20000000 of length 0x8000, and a metadata block at the end of the

binary containing the LOAD_MAP as the second Item (after the IMAGE_DEF, which means the LOAD_MAP is 8 bytes into the

block), then the relative LOAD_MAP would be:

0
1
0x06 (size_flag == 0, item_type == PICOBIN_BLOCK_ITEM_LOAD_MAP)

The absolute LOAD_MAP would be:

0
1
0x06 (size_flag == 0, item_type == PICOBIN_BLOCK_ITEM_LOAD_MAP)

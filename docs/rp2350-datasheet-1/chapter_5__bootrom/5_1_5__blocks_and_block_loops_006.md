---
source_pdf: rp2350-datasheet-1.pdf
repository: llm_database
chapter: Chapter 5. Bootrom
section: 5.1.5. Blocks and block loops
pages: 357-357
type: technical_spec
generated_at: 2026-02-28T17:22:56.644477+00:00
---

# 5.1.5. Blocks and block loops

For details about the IMAGE_DEF format itself, see Section 5.9.3.
For a description of the minimum requirements for a bootable image, see Section 5.9.5.
5.1.5. Blocks and block loops
5.1.5.1. Blocks
IMAGE_DEFs and PARTITION_TABLEs are both examples of blocks. A block is a recognisable, self-checking data structure
containing one or more distinct data items. The type of the first item in a block defines the type of that entire block.
Blocks are backwards and forwards compatible; item types will not be changed in the future in ways that could cause
existing code to misinterpret data. Consumers of blocks (including the bootrom) must skip items within the block
whose types are currently listed as reserved; encountering reserved item types must not cause a block to fail validation.
To be considered valid, a block must have the following properties:
• it must begin with the 4 byte magic header, PICOBIN_BLOCK_MARKER_START (0xffffded3)
• the end of each (variably-sized) item must also be the start of another valid item
• the last item must have type PICOBIN_BLOCK_ITEM_2BS_LAST and specify the correct full length of the block
• it must end with the 4 byte magic footer, PICOBIN_BLOCK_MARKER_END (0xab123579)
The magic header and footer values are chosen to be unlikely to appear in executable Arm and RISC-V code. For more
information about the block format, see Section 5.9.1.
Given a region of memory or flash (such as a partition), blocks are found by searching the first 4 kB of that given region
(for flash boot) or the entire region (for RAM/OTP image boots) for a valid block which is part of a valid block loop.
Currently IMAGE_DEFs and PARTITION_TABLEs are the only types of block used by the RP2350 bootrom, but the block format
reserves encoding space for future expansion.
5.1.5.2. Block loops
A block loop is a cyclic linked list of blocks (a linked loop). Each block has a relative pointer to the next block, and the
last block must link to the first. A single block can form a block loop by linking back to itself with a relative pointer of 0.
The first block in a loop must have the lowest address of all blocks in the loop.
The purpose of a block loop is threefold:
• to discover which blocks belong to the same image without a brute-force search
• to allow metadata to be appended in post-link processing steps
• to detect parts of the binary being overwritten in a way that breaks the loop
For flash image boot the bootrom searches the first 4 kB of flash; the 4 kB size is a compromise between allowing
flexibility for different languages' memory layouts, while avoiding scanning too much flash when trying different flash
access modes and QSPI clock frequencies. flash partition boot also limits its search to the first 4 kB of the partition.
The search window may be larger, such as a RAM image boot following a UF2 SRAM download, where the search
window is all of SRAM. For the fastest boot time, locate the first block as close to the beginning of the binary as
possible.
Block loops support multiple blocks because:
• Signing an image duplicates the existing IMAGE_DEF and adds another (bigger) IMAGE_DEF with additional signature
information.
• An image may contain multiple IMAGE_DEFs, for example, with different signing keys.
RP2350 Datasheet
5.1. Bootrom concepts
356


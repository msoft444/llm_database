---
source_pdf: rp2350-datasheet-4.pdf
repository: llm_database
chapter: Chapter 5. Bootrom
section: 5.9.1. Blocks and block loops
pages: 419-419
type: technical_spec
generated_at: 2026-03-01T03:27:49.792955+00:00
---

# 5.9.1. Blocks and block loops

5.9.1. Blocks and block loops

Blocks consist of a fixed 32-bit header, one or more items, a 32-bit relative offset to the next block, and a fixed 32-bit

footer. All multi-byte values within a block are little-endian. Blocks must start on a word-aligned boundary, and the total

size is always an exact number of words (a multiple of four bytes).

The final item in a block must be of type PICOBIN_BLOCK_ITEM_LAST, which encodes the total word count of the block’s

items.

The 32-bit relative link forms a linked list of blocks. To be valid, this linked list must eventually link back to the first block

in the list, forming a closed block loop; failure to close the loop results in the entire linked list being ignored. The loop

rule is used to avoid treating orphaned blocks from partially overwritten images being treated as valid.

Due to RAM restrictions in the boot path, size of blocks is limited to 640 bytes for PARTITION_TABLEs and 384 bytes for

IMAGE_DEFs. Blocks larger than this are ignored.

The format of a simple block with two items is shown:

![Page 419 figure](images/fig_p0419.png)

Item
Word
Bytes
Value

HEADER
0
4
0xffffded3

ITEM 0
1
1
size_flag:1 (0 means 1-byte size, 1 means 2-byte size), item_type:7

1
s0 / 256 if size_flag == 1 or type-specific data for blocks that are never > 256

ITEM 1
1 + s0
1
size_flag:1 (0 means 1-byte size, 1 means 2-byte size), item_type:7

1
s1 / 256 if size_flag == 1 or type-specific data for blocks that are never > 256

5.9. Metadata block details
418

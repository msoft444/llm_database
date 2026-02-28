---
source_pdf: rp2350-datasheet-1.pdf
repository: llm_database
chapter: Chapter 5. Bootrom
section: 5.9.1. Blocks and block loops
pages: 419-419
type: technical_spec
generated_at: 2026-02-28T17:22:56.644477+00:00
---

# 5.9.1. Blocks and block loops

5.8.5. Requirements for UART boot binaries
A UART boot binary is a normal RAM binary. It must have a valid IMAGE_DEF in order for the boot path to recognise it as a
bootable binary. The search window for the IMAGE_DEF is the whole of SRAM, but it’s recommended to place it close to
the beginning, because the bootrom searches linearly forward for the beginning of the IMAGE_DEF.
The maximum size for a UART boot binary is the entirety of main SRAM: 520 kB, or 532 480 bytes.
UART boot only supports loading to the start of SRAM, so your binary must be linked to run at address 0x20000000.
Sparse loading is unsupported. Your program must load as a single flat binary image.
All security requirements relating to RAM image boot apply to UART boot too. If secure boot is enabled, your binary
must be signed. Likewise, if OTP anti-rollback versioning is in effect, your binary’s rollback version must be no lower
than the version number stored in OTP.
5.9. Metadata block details
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
s0 % 256
1
s0 / 256 if size_flag == 1 or type-specific data for blocks that are never > 256
words
1
Type-specific data
…
…
…
ITEM 1
1 + s0
1
size_flag:1 (0 means 1-byte size, 1 means 2-byte size), item_type:7
1
s1 % 256
1
s1 / 256 if size_flag == 1 or type-specific data for blocks that are never > 256
words
1
Type-specific data
…
…
…
RP2350 Datasheet
5.9. Metadata block details
418


---
source_pdf: rp2350-datasheet-1.pdf
repository: llm_database
chapter: Chapter 5. Bootrom
section: 5.9.2. Common block items
pages: 420-421
type: technical_spec
generated_at: 2026-02-28T17:22:56.644477+00:00
---

# 5.9.2. Common block items

Item
Word
Bytes
Value
LAST_ITEM
1 + s0 + s1
1
0xff (size_flag == 1, item type == BLOCK_ITEM_LAST)
2
s1 + s2 (other items' size)
1
0x00 (pad)
LINK
2 + s0 + s1
4
Relative position in bytes of next block HEADER relative to this block’s HEADER. this
forms a loop, so a single block loop has 0 here.
FOOTER
3 + s0 + s1
4
0xab123579
IMAGE_DEF and PARTITION_TABLE blocks are recognised by their first item being an IMAGE_DEF or PARTITION_TABLE item.
Constants describing blocks can be found in the SDK in picobin.h in the SDK.
5.9.2. Common block items
The following items might appear in a IMAGE_DEF or a PARTITION_TABLE block.
5.9.2.1. VERSION item
A major/minor version number for the binary, 32 bits total, plus optionally a 16-bit rollback version and a list of OTP
rows which can be read to determine the (thermometer-coded) minimum major rollback version which this device will
allow to be installed. The major and minor are always present, whereas the rollback version and OTP row list are
generally only included if rollback protection is required.
NOTE
The rollback version and OTP row list are only valid for IMAGE_DEFs, and are ignored on a RP2350 that hasn’t been
secured.
If the number of OTP row entries is zero, there is no rollback version for this block.
Word
Bytes
Value
0
1
0x48 (size_flag == 0, item_type == PICOBIN_BLOCK_ITEM_1BS_VERSION)
1
2 + ((num_otp_row_entries != 0) + num_row_entries + 1) / 2
1
0x00 (pad)
1
num_otp_row_entries
1
2
Minor Version
2
Major Version
(2)
(2)
Rollback version (if num_otp_entries != 0)
(2)
First 16-bit OTP Row index (if num_otp_entries != 0`)
…
…
Remaining 16-OTP Row indexes (padded with a zero to make a word boundary if necessary)
Each OTP row entry indicates the row number (1 through 4095 inclusive) of the first in a group of 3 OTP rows. The three
OTP rows are each read as a 24 bit raw value, combined via a bitwise majority vote, and then the index of the most-
significant 1 bit determines the version number. So, a single group of three rows can encode rollback versions from 0 to
23 inclusive, or, when all 24 bits are set, an indeterminate version of at least 24. Each additional OTP row index indicates
a further group of 3 rows that increases the maximum version by 24.
There is no requirement for different OTP row entries to be contiguous in OTP. They should not overlap, though the
RP2350 Datasheet
5.9. Metadata block details
419


bootrom doesn’t need to check this (the boot signing tool may).
NOTE
For this entry to be considered valid, the number of available bits in the indicated OTP rows must be strictly greater
than the rollback version. This means that it is always possible to determine that the device’s minimum rollback
version is greater than the rollback version indicated in this block, even if we don’t know the full list of OTP rows
used by later major versions.
The major/minor version are used to disambiguate which is newer out of two binaries with the same major rollback
version. For example, to select which A/B image to boot from. when no major rollback version is specified, A/B
comparisons will treat the missing major version as zero, but no rollback check will be performed.
5.9.2.2. HASH_DEF item
Optional item with information about what how to hash:
Word
Bytes
Value
0
1
0x47 (size_flag == 0, item_type == PICOBIN_BLOCK_ITEM_1BS_HASH_DEF)
1
0x03 (size_lo)
1
0x00 (pad)
1
0x01 (PICOBIN_HASH_SHA-256)
1
2
Number of words of block hashed (not including HEADER word at the start of the block)
2
0x0000 (pad)
block_words_hashed must include this item if using this item for a signature.
The most recent LOAD_MAP item (see Section 5.9.3.2) that defines what to hash.
5.9.2.3. HASH_VALUE item
Optional item containing a hash value that can be used by the bootrom to verify the hash of an image or partition table
when not using signatures.
Word
Bytes
Value
0
1
0x09 (size_flag == 0, item_type == PICOBIN_BLOCK_ITEM_HASH_VALUE)
1
0x01 + n where n is the number of hash words included (1-8)
2
0x0000 (pad)
1
4
Hash Value (lowest significant 32 bits)
…
…
…
n
4
Hash Value (highest significant 32 bits)
RP2350 Datasheet
5.9. Metadata block details
420


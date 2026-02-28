---
source_pdf: rp2350-datasheet-1.pdf
repository: llm_database
chapter: Chapter 5. Bootrom
section: 5.1.8. Hashing and signing
pages: 358-358
type: technical_spec
generated_at: 2026-02-28T17:22:56.644477+00:00
---

# 5.1.8. Hashing and signing

• Placing a block at both the beginning and end of an image can detect some partial overwrites of the image (for
example, due to an overly enthusiastic absolute-addressed UF2 download). The SDK does this by default. Hashing
or signing the entire image is more robust, since it detects corruption in the middle of the image.
• A universal binary image might contain code for both Arm and RISC-V, including IMAGE_DEFs for both.
• PARTITION_TABLEs and IMAGE_DEFs are both present in the same block loop in the case of an embedded partition table.
If a block loop contains multiple IMAGE_DEFs or multiple PARTITION_TABLEs, the winner is generally the last one seen in
linked-list order. The exception is the case of two IMAGE_DEFs for different architectures (Arm and RISC-V); an IMAGE_DEF
for the architecture currently executing the bootrom is always preferred over one for a different architecture.
5.1.6. Block versioning
Any block may contain a version. Version information consists of a tuple of either two or three 16-bit values:
(rollback).major.minor, where the rollback part is optional. An item of type VERSION contains the binary data structure
which defines the version of a block.
The rollback version may only be specified for IMAGE_DEFs and defaults to zero if not present. You cannot specify this
version for partition tables. The rollback version can be used on a secured RP2350, where it, along with a current
rollback verson number stored in OTP, can prevent installation of older, vulnerable code once a newer version is
installed (Section 5.1.11).
The full version number can be used to pick the latest version between two IMAGE_DEFs or two PARTITION_TABLEs (see
Section 5.1.7). Versions compare in lexicographic order:
1. If version x has a different rollback version than version y, then the greater rollback version determines which
version is greater overall
2. Else if version x has a different major version than version y, then the greater major version determines which
version is greater overall
3. Else the minor version determines which of x and y is greater
See Section 5.9.2.1 for full details on the VERSION item in a block.
5.1.7. A/B versions
A pair of partitions may be grouped into an A/B pair. By logically grouping A and B partitions, you can keep the current
executable image (or data) in one partition, and write a newer version into the other partition. When you finish writing a
new version, you can safely switch to it, reverting to the older version if problems arise. This avoids partially written
states that could render RP2350 un-bootable.
• When booting an A/B partition pair, the bootrom typically uses the partition with the higher version. For scenarios
where this is not the case, see Section 5.1.16.
• When dragging a UF2 onto the BOOTSEL USB drive, the UF2 targets the opposite A/B partition to the one preferred
at boot. See Section 5.1.18 for more details.
NOTE
It is also possible to have A/B versions of the partition table. For more information about this advanced topic, see
Section 5.1.15.
5.1.8. Hashing and signing
Any block may be hashed or signed. A hashed block stores the image hash value (see Section 5.9.2.3). At runtime, the
bootrom calculates a hash and compares it to the stored hash to determine if the block is valid. Hashes guard against
RP2350 Datasheet
5.1. Bootrom concepts
357


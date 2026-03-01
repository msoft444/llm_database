---
source_pdf: rp2350-datasheet-4.pdf
repository: llm_database
chapter: Chapter 5. Bootrom
section: 5.1.2. Partition tables
pages: 355-355
type: technical_spec
generated_at: 2026-03-01T03:27:49.792955+00:00
---

# 5.1.2. Partition tables

5.1.2. Partition tables

A partition table divides flash into a maximum of 16 distinct regions, known as partitions. Each partition defines

attributes such as flash permissions for a contiguous range of flash addresses. The PARTITION_TABLE data structure

describes a partition table, and is an example of a block. Use of partition tables is strictly optional.

Dividing flash into multiple partitions enables you to:

• Store more than one executable image on the device. For example:

◦For A/B boot versions (Section 5.1.7)

◦For different architectures (Arm/RISC-V) or Secure/Non-secure

◦For use with a custom bootloader
• Provision space for data. For example:

◦Embedded file systems

◦Shared Wi-Fi firmware

◦Application resources
• Provide different security attributes for different regions of flash (Section 5.1.3)
• Target UF2 downloads to different partitions based on family ID (Section 5.1.18), including custom-defined UF2

families specific to your platform

For more information about PARTITION_TABLE discovery during flash boot, see Section 5.1.5.2.

Partition tables can be versioned to support A/B upgrades. They can also be hashed and signed for security and

integrity purposes. We recommend hashing partition tables to ensure that they haven’t been corrupted. This is

especially important when using boot slots to update your partition table, since a corrupted partition table with a higher

version could be chosen over a non-corrupted partition table with a lower version.

5.1. Bootrom concepts
354

---
source_pdf: rp2350-datasheet-6.pdf
repository: llm_database
chapter: Chapter 5. Bootrom
section: 5.1.7. A/B versions
pages: 358-358
type: technical_spec
generated_at: 2026-03-01T05:39:21.783275+00:00
---

# 5.1.7. A/B versions

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

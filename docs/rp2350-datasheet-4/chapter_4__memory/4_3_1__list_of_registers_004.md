---
source_pdf: rp2350-datasheet-4.pdf
repository: llm_database
chapter: Chapter 4. Memory
section: 4.3.1. List of registers
pages: 340-340
type: technical_spec
generated_at: 2026-03-01T03:27:49.792955+00:00
---

# 4.3.1. List of registers

4.3.1. List of registers

A small number of registers are located on the same bus endpoint as boot RAM:

Write Once Bits

These are flags which once set, can only be cleared by a system reset. They are used in the implementation of

certain bootrom security features.

Boot Locks

These function the same as the SIO spinlocks (Section 3.1.4), however they are normally reserved for bootrom

purposes (Section 5.4.4).

These registers start from an offset of 0x800 above the boot RAM base address of 0x400e0000 (defined as

BOOTRAM_BASE in the SDK).

| Offset | Name | Info |
| --- | --- | --- |
| 0x800 | WRITE_ONCE0 | This registers always ORs writes into its current contents. Once a bit is set, it can only be cleared by a reset. |
| 0x804 | WRITE_ONCE1 | This registers always ORs writes into its current contents. Once a bit is set, it can only be cleared by a reset. |
| 0x808 | BOOTLOCK_STAT | Bootlock status register. 1=unclaimed, 0=claimed. These locks function identically to the SIO spinlocks, but are reserved for bootrom use. |

Table 435. List of

4.3. Boot RAM
339

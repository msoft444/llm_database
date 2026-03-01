---
source_pdf: rp2350-datasheet-6.pdf
repository: llm_database
chapter: Chapter 4. Memory
section: 4.3.1. List of registers
pages: 340-341
type: technical_spec
generated_at: 2026-03-01T05:39:21.783275+00:00
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
| 0x80c | BOOTLOCK0 | Read to claim and check. Write to unclaim. The value returned on successful claim is 1 << n, and on failed claim is zero. |
| 0x810 | BOOTLOCK1 | Read to claim and check. Write to unclaim. The value returned on successful claim is 1 << n, and on failed claim is zero. |
| 0x814 | BOOTLOCK2 | Read to claim and check. Write to unclaim. The value returned on successful claim is 1 << n, and on failed claim is zero. |
| 0x818 | BOOTLOCK3 | Read to claim and check. Write to unclaim. The value returned on successful claim is 1 << n, and on failed claim is zero. |
| 0x81c | BOOTLOCK4 | Read to claim and check. Write to unclaim. The value returned on successful claim is 1 << n, and on failed claim is zero. |
| 0x820 | BOOTLOCK5 | Read to claim and check. Write to unclaim. The value returned on successful claim is 1 << n, and on failed claim is zero. |
| 0x824 | BOOTLOCK6 | Read to claim and check. Write to unclaim. The value returned on successful claim is 1 << n, and on failed claim is zero. |
| 0x828 | BOOTLOCK7 | Read to claim and check. Write to unclaim. The value returned on successful claim is 1 << n, and on failed claim is zero. |

Table 435. List of

4.3. Boot RAM
339

RP2350 Datasheet

BOOTRAM: WRITE_ONCE0, WRITE_ONCE1 Registers

Offsets: 0x800, 0x804

| Bits | Description | Type | Reset |
| --- | --- | --- | --- |
| 31:0 | This registers always ORs writes into its current contents. Once a bit is set, it can only be cleared by a reset. | RW | 0x00000000 |

Table 436.

WRITE_ONCE0,

WRITE_ONCE1

Registers

BOOTRAM: BOOTLOCK_STAT Register

Offset: 0x808

| Bits | Description | Type | Reset |
| --- | --- | --- | --- |
| 31:8 | Reserved. | - | - |
| 7:0 | Bootlock status register. 1=unclaimed, 0=claimed. These locks function identically to the SIO spinlocks, but are reserved for bootrom use. | RW | 0xff |

Table 437.

BOOTLOCK_STAT

Register

BOOTRAM: 
BOOTLOCK0, 
BOOTLOCK1, 
…, 
BOOTLOCK6, 
BOOTLOCK7
Registers

Offsets: 0x80c, 0x810, …, 0x824, 0x828

| Bits | Description | Type | Reset |
| --- | --- | --- | --- |
| 31:0 | Read to claim and check. Write to unclaim. The value returned on successful claim is 1 << n, and on failed claim is zero. | RW | 0x00000000 |

Table 438.

BOOTLOCK0,

BOOTLOCK1, …,

BOOTLOCK6,

BOOTLOCK7 Registers

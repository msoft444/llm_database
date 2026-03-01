---
source_pdf: rp2350-datasheet-3.pdf
repository: llm_database
chapter: Chapter 4. Memory
section: 4.4. External flash and PSRAM (XIP)
pages: 341-341
type: technical_spec
generated_at: 2026-03-01T02:30:35.833805+00:00
---

# 4.4. External flash and PSRAM (XIP)

![Page 341 figure](images/fig_p0341.png)

RP2350 Datasheet

| Offset | Name | Info |
| --- | --- | --- |
| 0x80c | BOOTLOCK0 | Read to claim and check. Write to unclaim. The value returned on successful claim is 1 << n, and on failed claim is zero. |
| 0x810 | BOOTLOCK1 | Read to claim and check. Write to unclaim. The value returned on successful claim is 1 << n, and on failed claim is zero. |
| 0x814 | BOOTLOCK2 | Read to claim and check. Write to unclaim. The value returned on successful claim is 1 << n, and on failed claim is zero. |
| 0x818 | BOOTLOCK3 | Read to claim and check. Write to unclaim. The value returned on successful claim is 1 << n, and on failed claim is zero. |
| 0x81c | BOOTLOCK4 | Read to claim and check. Write to unclaim. The value returned on successful claim is 1 << n, and on failed claim is zero. |
| 0x820 | BOOTLOCK5 | Read to claim and check. Write to unclaim. The value returned on successful claim is 1 << n, and on failed claim is zero. |
| 0x824 | BOOTLOCK6 | Read to claim and check. Write to unclaim. The value returned on successful claim is 1 << n, and on failed claim is zero. |
| 0x828 | BOOTLOCK7 | Read to claim and check. Write to unclaim. The value returned on successful claim is 1 << n, and on failed claim is zero. |

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

4.4. External flash and PSRAM (XIP)

RP2350 can access external flash and PSRAM via its execute-in-place (XIP) subsystem. The term execute-in-place

refers to external memory mapped directly into the chip’s internal address space. This enables you to execute code as-

4.4. External flash and PSRAM (XIP)
340

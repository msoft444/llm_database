---
source_pdf: rp2350-datasheet-3.pdf
repository: llm_database
chapter: Chapter 4. Memory
section: 4.2.1. Other on-chip memory
pages: 339-339
type: technical_spec
generated_at: 2026-03-01T02:30:35.833805+00:00
---

# 4.2.1. Other on-chip memory

![Page 339 figure](images/fig_p0339.png)

RP2350 Datasheet

| System address | SRAM Bank | SRAM word address |
| --- | --- | --- |
| 0x20000000 | Bank 0 | 0 |
| 0x20000004 | Bank 1 | 0 |
| 0x20000008 | Bank 2 | 0 |
| 0x2000000c | Bank 3 | 0 |
| 0x20000010 | Bank 0 | 1 |
| 0x20000014 | Bank 1 | 1 |
| 0x20000018 | Bank 2 | 1 |
| 0x2000001c | Bank 3 | 1 |
| 0x20000020 | Bank 0 | 2 |
| 0x20000024 | Bank 1 | 2 |
| 0x20000028 | Bank 2 | 2 |
| 0x2000002c | Bank 3 | 2 |
| etc |  |  |

Table 434. SRAM

bank0/1/2/3 striped

mapping.

The top two 4 kB regions (starting at 0x20080000 and 0x20081000) map directly to the smaller 4 kB memory banks.

Software may choose to use these for per-core purposes (e.g. stack and frequently-executed code), guaranteeing that

the processors never stall on these accesses. Like all SRAM on RP2350, these banks have single-cycle access from all

managers, (provided no other managers access the bank in the same cycle) so it is reasonable to treat memory as a

single 520 kB device.

NOTE

RP2040 had a non-striped SRAM mirror. RP2350 no longer has a non-striped mirror, to avoid mapping the same

SRAM location as both Secure and Non-secure. You can still achieve some explicit bandwidth partitioning by

allocating data across two 256 kB blocks of 4-way-striped SRAM.

4.2.1. Other on-chip memory

Besides the 520 kB main memory, there are two other dedicated RAM blocks that may be used in some circumstances:

• Cache lines can be individually pinned within the XIP address space for use as SRAM, up to the total cache size of

16 kB (see Section 4.4.1.3). Unpinned cache lines remain available for transparent caching of XIP accesses.
• If USB is not used, the USB data DPRAM can be used as a 4 kB memory starting at 0x50100000.

There is also 1 kB of dedicated boot RAM, hardwired to Secure access only, whose contents and layout is defined by the

bootrom — see Chapter 5.

4.2. SRAM
338

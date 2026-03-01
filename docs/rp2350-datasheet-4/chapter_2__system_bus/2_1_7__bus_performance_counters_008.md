---
source_pdf: rp2350-datasheet-4.pdf
repository: llm_database
chapter: Chapter 2. System bus
section: 2.1.7. Bus performance counters
pages: 31-31
type: technical_spec
generated_at: 2026-03-01T03:27:49.792955+00:00
---

# 2.1.7. Bus performance counters

2.1.7. Bus performance counters

Bus performance counters automatically count accesses to the main AHB5 crossbar arbiters. These counters can help

diagnose high-traffic performance issues.

There are four performance counters, starting at PERFCTR0. Each is a 24-bit saturating counter. Counter values can be

read from BUSCTRL_PERFCTRx and cleared by writing any value to BUSCTRL_PERFCTRx. Each counter can count one of the 20

available events at a time, as selected by BUSCTRL_PERFSELx. For more information, see Section 12.15.4.

2.2. Address map

The address map for the device is split into sections as shown in Table 8. Details are shown in the following sections.

Unmapped address ranges raise a bus error when accessed.

Each link in the left-hand column of Table 8 goes to a detailed address map for that address range. The detailed

address maps have a link for each address to the relevant documentation for that address.

Rough address decode is first performed on bits 31:28 of the address:

| Bus Segment | Base Address |
| --- | --- |
| ROM | 0x00000000 |
| XIP | 0x10000000 |
| SRAM | 0x20000000 |
| APB Peripherals | 0x40000000 |
| AHB Peripherals | 0x50000000 |
| Core-local Peripherals (SIO) | 0xd0000000 |
| Cortex-M33 private registers | 0xe0000000 |

Table 8. Address Map

![Page 31 figure](images/fig_p0031.png)

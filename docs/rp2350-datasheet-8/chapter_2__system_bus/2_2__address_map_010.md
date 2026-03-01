---
source_pdf: rp2350-datasheet-8.pdf
repository: llm_database
chapter: Chapter 2. System bus
section: 2.2. Address map
pages: 31-31
type: technical_spec
generated_at: 2026-03-01T08:45:43.944040+00:00
---

# 2.2. Address map

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

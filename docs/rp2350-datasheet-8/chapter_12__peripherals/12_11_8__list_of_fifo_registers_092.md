---
source_pdf: rp2350-datasheet-8.pdf
repository: llm_database
chapter: Chapter 12. Peripherals
section: 12.11.8. List of FIFO registers
pages: 1213-1213
type: technical_spec
generated_at: 2026-03-01T08:45:43.944040+00:00
---

# 12.11.8. List of FIFO registers

12.11.8. List of FIFO registers

The FIFO registers start at a base address of 0x50600000 (defined as HSTX_FIFO_BASE in the SDK).

| Offset | Name | Info |
| --- | --- | --- |
| 0x0 | STAT | FIFO status |
| 0x4 | FIFO | Write access to FIFO |

Table 1258. List of

HSTX_FIFO: STAT Register

Offset: 0x0

Description

FIFO status

| Bits | Description | Type | Reset |
| --- | --- | --- | --- |
| 31:11 | Reserved. | - | - |
| 10 | WOF: FIFO was written when full. Write 1 to clear. | WC | 0x0 |
| 9 | EMPTY | RO | - |
| 8 | FULL | RO | - |
| 7:0 | LEVEL | RO | 0x00 |

Table 1259. STAT

HSTX_FIFO: FIFO Register

Offset: 0x4

| Bits | Description | Type | Reset |
| --- | --- | --- | --- |
| 31:0 | Write access to FIFO | WF | 0x00000000 |

Table 1260. FIFO

12.12. TRNG

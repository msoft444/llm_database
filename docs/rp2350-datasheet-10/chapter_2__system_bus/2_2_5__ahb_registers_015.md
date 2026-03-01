---
source_pdf: rp2350-datasheet-10.pdf
repository: llm_database
chapter: Chapter 2. System bus
section: 2.2.5. AHB registers
pages: 34-34
type: technical_spec
generated_at: 2026-03-01T12:20:14.494106+00:00
---

# 2.2.5. AHB registers

2.2.5. AHB registers

AHB peripheral registers are accessible to processor load/store and DMA only. Instruction fetch will always fail.

The AHB peripheral segment provides access to higher-bandwidth peripherals. The minimum read/write cost is one

cycle, and peripherals may insert up to one wait state.

| Bus Endpoint | Base Address |
| --- | --- |
| DMA_BASE | 0x50000000 |
| USBCTRL_BASE | 0x50100000 |
| USBCTRL_DPRAM_BASE | 0x50100000 |
| USBCTRL_REGS_BASE | 0x50110000 |
| PIO0_BASE | 0x50200000 |
| PIO1_BASE | 0x50300000 |
| PIO2_BASE | 0x50400000 |
| XIP_AUX_BASE | 0x50500000 |
| HSTX_FIFO_BASE | 0x50600000 |
| CORESIGHT_TRACE_BASE | 0x50700000 |

*Table 14. Address map for AHB peripheral bus segment*

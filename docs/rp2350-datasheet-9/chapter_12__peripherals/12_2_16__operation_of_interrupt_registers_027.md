---
source_pdf: rp2350-datasheet-9.pdf
repository: llm_database
chapter: Chapter 12. Peripherals
section: 12.2.16. Operation of interrupt registers
pages: 1009-1009
type: technical_spec
generated_at: 2026-03-01T11:44:31.548839+00:00
---

# 12.2.16. Operation of interrupt registers

12.2.16. Operation of interrupt registers

Table 1054 lists the operation of the DW_apb_i2c interrupt registers and how they are set and cleared. Some bits are set

by hardware and cleared by software, whereas other bits are set and cleared by hardware.

| Interrupt Bit Fields | Set by Hardware/Cleared by Software | Set and Cleared by Hardware |
| --- | --- | --- |
| RESTART_DET | Y | N |
| GEN_CALL | Y | N |
| START_DET | Y | N |
| STOP_DET | Y | N |
| ACTIVITY | Y | N |
| RX_DONE | Y | N |
| TX_ABRT | Y | N |
| RD_REQ | Y | N |
| TX_EMPTY | N | Y |
| TX_OVER | Y | N |
| RX_FULL | N | Y |
| RX_OVER | Y | N |
| RX_UNDER | Y | N |

*Table 1054. Clearing and Setting of Interrupt Registers*

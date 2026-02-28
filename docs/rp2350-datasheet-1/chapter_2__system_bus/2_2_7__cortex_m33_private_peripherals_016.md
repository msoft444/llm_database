---
source_pdf: rp2350-datasheet-1.pdf
repository: llm_database
chapter: Chapter 2. System bus
section: 2.2.7. Cortex-M33 private peripherals
pages: 35-35
type: technical_spec
generated_at: 2026-02-28T17:22:56.644477+00:00
---

# 2.2.7. Cortex-M33 private peripherals

Table 15. Address
map for SIO bus
segment
Bus Endpoint
Base Address
SIO_BASE
0xd0000000
SIO_NONSEC_BASE
0xd0020000
2.2.7. Cortex-M33 private peripherals
The PPB is accessible to processor load/store only.
The PPB region contains standard control registers defined by Arm, Non-secure aliases of some of those registers, and
a handful of other core-local registers defined by Raspberry Pi (the EPPB).
These addresses are only accessible to Arm processors: RISC-V processors will return a bus fault.
Table 16. Address
map for PPB bus
segment
Bus Endpoint
Base Address
PPB_BASE
0xe0000000
PPB_NONSEC_BASE
0xe0020000
EPPB_BASE
0xe0080000
RP2350 Datasheet
2.2. Address map
34


---
source_pdf: rp2350-datasheet-10.pdf
repository: llm_database
chapter: Chapter 10. Security
section: 10.7.2. Memory protection unit
pages: 869-869
type: technical_spec
generated_at: 2026-03-01T12:20:14.494106+00:00
---

# 10.7.2. Memory protection unit

10.7.2. Memory protection unit

The RP2350 DMA features a memory protection unit that you can configure to set the security/privilege level required to

access up to eight different address ranges, plus a default level for addresses not matched by any of those eight

ranges. The addresses of all DMA reads and writes are checked against the MPU address map. If the originating

channel’s security level is lower than that defined in the address map, the access is filtered. A filtered access has no

effect on the downstream bus, and returns a bus error to the offending channel.

The DMA memory protection unit is configured by DMA control registers starting from MPU_CTRL. See Section 12.6.6.3

for more details.

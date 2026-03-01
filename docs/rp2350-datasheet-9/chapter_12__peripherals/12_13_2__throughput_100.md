---
source_pdf: rp2350-datasheet-9.pdf
repository: llm_database
chapter: Chapter 12. Peripherals
section: 12.13.2. Throughput
pages: 1223-1223
type: technical_spec
generated_at: 2026-03-01T11:44:31.548839+00:00
---

# 12.13.2. Throughput

12.13.2. Throughput

SHA-256 processes data one 512-bit block at a time. This requires 16 32-bit writes, 32 16-bit writes, or 64 8-bit writes to

the WDATA register. An APB register write costs 4 cycles, so it takes at least 64 system clock cycles to write a data

block.

Once a full block is transferred, the SHA core takes a further 57 cycles to complete the block digest. CSR.WDATA_RDY

goes low, and you must not write to WDATA during this time.

The maximum throughput is therefore one block per 121 system clock cycles, or 0.53 bytes per cycle. At a 150 MHz

system clock this is 79.3 MB/s. This throughput is achieved when you use 32-bit transfers from the DMA. Using

narrower transfers result in lower throughput, as does polling the CSR.WDATA_RDY flag when transferring data from the

processor.

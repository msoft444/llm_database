---
source_pdf: rp2350-datasheet-8.pdf
repository: llm_database
chapter: Chapter 12. Peripherals
section: 12.13.4. DMA DREQ interface
pages: 1223-1224
type: technical_spec
generated_at: 2026-03-01T08:45:43.944040+00:00
---

# 12.13.4. DMA DREQ interface

12.13.4. DMA DREQ interface

The block can request the DMA controller to send entire blocks of data at once. Configure transfer size using

12.13. SHA-256 accelerator
1222

RP2350 Datasheet

CSR.DMA_SIZE so that the DMA controller requests the correct number of transfers.

The DREQ always requests one full SHA block of data at a time. Do not start a DMA on a non-block boundary.

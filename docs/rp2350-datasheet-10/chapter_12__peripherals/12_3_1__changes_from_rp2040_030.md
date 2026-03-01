---
source_pdf: rp2350-datasheet-10.pdf
repository: llm_database
chapter: Chapter 12. Peripherals
section: 12.3.1. Changes from RP2040
pages: 1048-1048
type: technical_spec
generated_at: 2026-03-01T12:20:14.494106+00:00
---

# 12.3.1. Changes from RP2040

RP2350 Datasheet

12.3.1. Changes from RP2040

The output enable of the SSPTXD data output (connecting to pins listed as SPI0 TX and SPI1 TX in the GPIO function

tables) is controlled by the SPI peripheral nSSPOE signal. The peripheral automatically tristates its output when

deselected in slave mode. This makes software control of the output enable unnecessary even when multiple slaves

share the data lines.

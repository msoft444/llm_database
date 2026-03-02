---
source_pdf: rp2350-datasheet.pdf
repository: llm_database
chapter: Chapter 12. Peripherals
section: 12.4.1. Changes from RP2040
pages: 1069-1070
type: technical_spec
generated_at: 2026-03-02T14:11:45.471392+00:00
---

# 12.4.1. Changes from RP2040

• Removed spikes in differential nonlinearity at codes 0x200, 0x600, 0xa00 and 0xe00, as documented by erratum

RP2040-E11, improving the ADC’s precision by around 0.5 ENOB.

• Increased the number of external ADC input channels from 4 to 8 channels, in the QFN-80 package only.

---
source_pdf: rp2350-datasheet-4.pdf
repository: llm_database
chapter: Chapter 12. Peripherals
section: 12.6.1. Changes from RP2040
pages: 1096-1096
type: technical_spec
generated_at: 2026-03-01T03:27:49.792955+00:00
---

# 12.6.1. Changes from RP2040

![Page 1096 figure](images/fig_p1096.png)

12.6.1. Changes from RP2040

The following new features have been added:

• Increased the number of DMA channels from 12 to 16.
• Increased the number of shared IRQ outputs from 2 to 4.
• Channels can be assigned to security domains using SECCFG_CH0 through SECCFG_CH15.
• The DMA now filters bus accesses using the built-in memory protection unit (Section 12.6.6.3).
• Interrupts can be assigned to security domains using SECCFG_IRQ0 through SECCFG_IRQ3.
• Pacing timers and the CRC sniffer can be assigned to security domains using the SECCFG_MISC register.
• The four most-significant bits of TRANS_COUNT (CH0_TRANS_COUNT) are redefined as the MODE field, which defines

what happens when TRANS_COUNT reaches zero:

12.6. DMA
1095

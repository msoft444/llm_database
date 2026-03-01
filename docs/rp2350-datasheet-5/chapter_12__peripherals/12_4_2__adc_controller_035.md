---
source_pdf: rp2350-datasheet-5.pdf
repository: llm_database
chapter: Chapter 12. Peripherals
section: 12.4.2. ADC controller
pages: 1070-1070
type: technical_spec
generated_at: 2026-03-01T04:13:03.328193+00:00
---

# 12.4.2. ADC controller

12.4.2. ADC controller

A digital controller manages the details of operating the RP2350 ADC, and provides additional functionality:

• One-shot or free-running capture mode
• Sample FIFO with DMA interface
• Pacing timer (16 integer bits, 8 fractional bits) for setting free-running sample rate
• Round-robin sampling of multiple channels in free-running capture mode
• Optional right-shift to 8 bits in free-running capture mode, so samples can be DMA’d to a byte buffer in system

memory

12.4.2.1. Channel connections

The ADC channels are connected to the following GPIOs in QFN-60

| Channel | Connection |
| --- | --- |
| 0 | GPIO[26] |
| 1 | GPIO[27] |
| 2 | GPIO[28] |
| 3 | GPIO[29] |
| 4 | Temperature Sensor |

Table 1118. ADC

channel connections

on QFN-60

The ADC channels are connected to the following GPIOs in QFN-80

| Channel | Connection |
| --- | --- |
| 0 | GPIO[40] |
| 1 | GPIO[41] |
| 2 | GPIO[42] |
| 3 | GPIO[43] |
| 4 | GPIO[44] |
| 5 | GPIO[45] |
| 6 | GPIO[46] |
| 7 | GPIO[47] |
| 8 | Temperature Sensor |

Table 1119. ADC

channel connections

on QFN-80

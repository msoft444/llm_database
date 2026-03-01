---
source_pdf: rp2350-datasheet-8.pdf
repository: llm_database
chapter: Chapter 5. Bootrom
section: 5.8. UART boot
pages: 417-417
type: technical_spec
generated_at: 2026-03-01T08:45:43.944040+00:00
---

# 5.8. UART boot

RP2350 Datasheet

5.8. UART boot

UART boot is a minimal interface for bootstrapping a flashless RP2350 from a simple host, such as another

microcontroller. It is available by default on a blank device, so it allows RP2350 to be deployed into the field on multi-

device boards without loading firmware or programming OTP bits in advance.

To select UART boot, drive QSPI CSn low (BOOTSEL mode) and drive QSPI SD1 high. The bootrom checks these signals

shortly after device reset is released. UART TX appears on QSPI SD2, and UART RX appears on QSPI SD3.

The UART mode is 8n1: one start bit, eight data bits, no parity, one stop bit. Data within each UART frame is sent and

received LSB-first. The baud rate is fixed at 1 Mbaud.

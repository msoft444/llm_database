---
source_pdf: rp2350-datasheet-4.pdf
repository: llm_database
chapter: Chapter 5. Bootrom
section: 5.2.8. BOOTSEL (USB/UART) boot
pages: 375-375
type: technical_spec
generated_at: 2026-03-01T03:27:49.792955+00:00
---

# 5.2.8. BOOTSEL (USB/UART) boot

5.2.8. BOOTSEL (USB/UART) boot

The bootrom samples the state of QSPI CSn shortly after reset. Based on the result, the bootrom decides whether to

enter BOOTSEL mode, which refers collectively to the USB and UART bootloaders.

The bootrom initialises the chip select to the following state:

• Output disabled
• Pulled high (note CSn is an active-low signal, so this deselects the external QSPI device if there is one)

If the chip select remains high, the bootrom continues with its normal, non-BOOTSEL sequence. By default on a blank

device, this means driving the chip select low and attempting to boot from an external flash or PSRAM device.

If chip select is driven low externally, the bootrom enters BOOTSEL mode. You must drive the chip select low with a

5.2. Processor-controlled boot sequence
374

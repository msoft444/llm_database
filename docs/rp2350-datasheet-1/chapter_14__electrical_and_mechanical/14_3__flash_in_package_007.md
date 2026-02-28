---
source_pdf: rp2350-datasheet-1.pdf
repository: llm_database
chapter: Chapter 14. Electrical and mechanical
section: 14.3. Flash in package
pages: 1331-1331
type: technical_spec
generated_at: 2026-02-28T17:22:56.644477+00:00
---

# 14.3. Flash in package

Figure 146.
Recommended PCB
Footprint for the
RP2350 QFN-80
package
14.3. Flash in package
RP2354A and RP2354B feature 2 MB of internal flash. In all other respects, including pinout, they are identical to their
flashless counterparts RP2350A and RP2350B. They use the same QFN-60 (RP2354A) and QFN-80 (RP2354B)
packages. An RP2354 device contains two stacked silicon die:
• the same RP2350 die as the flashless variants
• a Winbond W25Q16JVWI QSPI NOR flash
Winbond W25Q16JVWI datasheet
For detailed information on the W25Q16JVWI device used in RP2354 see:
www.winbond.com/hq/product/code-storage-flash-memory/serial-nor-
flash/?__locale=en&partNo=W25Q16JV
The six dedicated QSPI pads on the RP2350 die (CSn, SCK and SD0 through SD3) connect to both the internal flash die and
the external package pins. This makes them behave similarly to flashless RP2350 devices in the following ways:
• The QSPI CSn can be driven low at reset or power-up to select BOOTSEL mode
◦This harmlessly selects the internal flash die but does not issue commands to it
• The QSPI SD1 pin can be driven high when selecting BOOTSEL to choose UART boot
◦UART TX appears on SD2 and UART RX on SD3, as per Section 5.8
◦Even with the chip select asserted low, the internal flash die maintains a high-impedance state on its SD0
through SD3 pins if there are no transitions on SCK, so you can keep CSn asserted throughout UART boot
• Internal flash can be programmed via UF2 drag-and-drop download using the USB BOOTSEL mode
• A second QSPI device can be attached externally by connecting it to the QSPI pins and a secondary chip select
from the Bank 0 GPIOs
◦This may be used for additional flash capacity, or external QSPI RAM
RP2350 Datasheet
14.3. Flash in package
1330


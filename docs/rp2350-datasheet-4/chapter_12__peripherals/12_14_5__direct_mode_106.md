---
source_pdf: rp2350-datasheet-4.pdf
repository: llm_database
chapter: Chapter 12. Peripherals
section: 12.14.5. Direct mode
pages: 1236-1236
type: technical_spec
generated_at: 2026-03-01T03:27:49.792955+00:00
---

# 12.14.5. Direct mode

12.14.5. Direct mode

In direct mode, the AHB XIP address window is disconnected from the QSPI bus, and the bus is controlled through a

TX/RX FIFO pair, similar to a normal SPI peripheral. In this state, the XIP window becomes inaccessible. Attempting to

access it generates a bus fault. This mode is used for low-level access to the QSPI bus, for example when issuing flash

erase/programming commands, or when accessing QSPI device status registers.

All direct-mode operation is controlled through DIRECT_CSR, with data being exchanged through DIRECT_TX and

DIRECT_RX. To enable direct mode, first set DIRECT_CSR.EN, and then poll for DIRECT_CSR.BUSY to go low to ensure

that any in-progress XIP transfer at the point direct mode was enabled has completed.

Direct mode has its own clock divisor and RX sampling delay, configured by DIRECT_CSR.CLKDIV and

DIRECT_CSR.RXDELAY. These are separate from the per-window settings configured in M0_TIMING/M1_TIMING,

because serial commands used for control purposes may have different frequency limits than data accesses used for

execute-in-place.

For each push to DIRECT_TX, QMI will issue 8 or 16 bits of FIFO data to the QSPI bus. Optionally, the same number of

bits are simultaneously sampled and returned in DIRECT_RX. The clock is initially low, and data is always captured on

the rising edge of SCK, transitioning on the subsequent falling edge.

After pushing to DIRECT_TX, DIRECT_CSR.BUSY will go high, and remain high until all direct-mode activity has

completed. This works even if no RX data is returned, so is more reliable than polling the RX FIFO status. The BUSY flag

12.14. QSPI memory interface (QMI)
1235

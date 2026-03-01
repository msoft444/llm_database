---
source_pdf: rp2350-datasheet-5.pdf
repository: llm_database
chapter: Chapter 12. Peripherals
section: 12.14.5. Direct mode
pages: 1236-1237
type: technical_spec
generated_at: 2026-03-01T04:13:03.328193+00:00
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

RP2350 Datasheet

stays high for half an SCK period after the transfer finishes, to ensure safe chip select timing when this is used to drive

the chip selects — see Section 12.14.5.2.

QMI will never push to a full RX FIFO, or drop data as a result of the FIFO being full — instead, the interface is paused

until the system pops DIRECT_RX. This avoids a common trap of RX data being lost when the processor is heavily

interrupted during direct-mode operation, but software must take care not to poll for DIRECT_CSR.BUSY low without

also checking the RX FIFO, as this can cause a deadlock when the FIFO fills.

12.14.5.1. Controls in DIRECT_TX

The TX FIFO carries control information as well as data, with data in the 16 LSBs, and control information in the

immediately more-significant bits:

• DIRECT_TX.NOPUSH inhibits the DIRECT_RX push which would match this TX data. This avoids creating garbage

when pushing control/address information at the start of a transfer.
• DIRECT_TX.DWIDTH is the data width of this FIFO record. 0 means the 8 LSBs contain data, and 1 means the 16

LSBs contain data. This also determines the amount of data returned in the matching DIRECT_RX entry.
• DIRECT_TX.IWIDTH is the interface width (single-dual/quad) used to clock out this FIFO record. The corresponding

RX data is sampled at the same width.
• DIRECT_TX.OE controls the pad direction for bidirectional transfers. It is ignored for serial IWIDTH, since SD0 is

always an output and SD1 always an input. At dual/quad width, it must be set in order to enable the output drivers

for the duration of this FIFO record. The TX data is don’t-care when IWIDTH is dual/quad and OE is not set.

The default when all control bits are zero is an 8-bit serial transfer, with 8 bits of sampled data returned. Therefore, you

can ignore the control bits and treat this as a plain 8-bit data FIFO.

12.14.5.2. Chip select control

There are two options for driving the chip selects, both via DIRECT_CSR:

• DIRECT_CSR.ASSERT_CS0N and DIRECT_CSR.ASSERT_CS1N will immediately drive the corresponding chip select

low when set
• DIRECT_CSR.AUTO_CS0N and DIRECT_CSR.AUTO_CS1N configure the corresponding chip select to be set low

whenever the interface is busy, i.e. when the DIRECT_CSR.BUSY flag is high due to a previous DIRECT_TX push

IMPORTANT

![Page 1237 figure](images/fig_p1237.png)

The ASSERT_CSxN fields assert the chip select unconditionally, including when DIRECT_CSR.EN is clear. Software must

take care not to set these fields when XIP transfers may be active.

---
source_pdf: rp2350-datasheet-1.pdf
repository: llm_database
chapter: Chapter 4. Memory
section: 4.4.3. Streaming DMA interface
pages: 346-346
type: technical_spec
generated_at: 2026-02-28T17:22:56.644477+00:00
---

# 4.4.3. Streaming DMA interface

4.4.2. QSPI Memory Interface (QMI)
Uncached accesses and cache misses require access to external memory. The QSPI memory interface (QMI) provides
this access, as documented in Section 12.14. The QMI supports:
• Up to two external QSPI devices, with separate chip selects and shared clock/data pins
◦Banked configuration registers, including different SCK frequencies and QSPI opcodes
• Memory-mapped reads and writes (writes must be enabled via CTRL.WRITABLE_M0/CTRL.WRITABLE_M1)
• Serial/dual/quad-SPI transfer formats
• SCK speeds as high as clk_sys
• 8/16/32-bit accesses for uncached accesses, and 64-bit accesses for cache line fills
• Automatic chaining of sequentially addressed accesses into a single QSPI transfer
• Address translation (4 × 4 MB windows per QSPI device)
◦Flash storage addresses can differ from runtime addresses, e.g. for multiple OTA upgrade image slots
◦Allows code and data segments, or Secure and Non-secure images, to be mapped separately
• Direct-mode FIFO interface for programming and configuring external QSPI devices
XIP accesses via the two cache AHB ports, and from the DMA streaming hardware, arbitrate for access to the QMI. A
separate APB port configures the QMI.
The QMI is a new memory interface designed for RP2350, replacing the SSI peripheral on RP2040.
4.4.3. Streaming DMA interface
As the flash is generally much larger than on-chip SRAM, it’s often useful to stream chunks of data into memory from
flash. It’s convenient to have the DMA stream this data in the background while software in the foreground does other
things. It’s even more convenient if code can continue to execute from flash whilst this takes place.
This doesn’t interact well with standard XIP operation because QMI serial transfers force lengthy bus stalls on the DMA.
These stalls are tolerable for a processor because an in-order processor tends to have nothing better to do while
waiting for an instruction fetch to retire, and because typical code execution tends to have much higher cache hit rates
than bulk streaming of infrequently accessed data. In contrast, stalling the DMA prevents any other active DMA
channels from making progress during this time, slowing overall DMA throughput.
The STREAM_ADDR and STREAM_CTR registers are used to program a linear sequence of flash reads. The XIP
subsystem performs these reads in the background in a best-effort fashion. To minimise impact on code executed from
flash whilst the stream is ongoing, the streaming hardware has lower priority access to the QMI than regular XIP
accesses, and there is a brief cooldown (9 cycles) between the last XIP cache miss and resuming streaming. This
avoids increases in initial access latency on XIP cache misses.
Pico Examples: https://github.com/raspberrypi/pico-examples/blob/master/flash/xip_stream/flash_xip_stream.c Lines 45 - 48
45     while (!(xip_ctrl_hw->stat & XIP_STAT_FIFO_EMPTY))
46         (void) xip_ctrl_hw->stream_fifo;
47     xip_ctrl_hw->stream_addr = (uint32_t) &random_test_data[0];
48     xip_ctrl_hw->stream_ctr = count_of(random_test_data);
The streamed data is pushed to a small FIFO, which generates DREQ signals that tell the DMA to collect the streamed
data. As the DMA does not initiate a read until after reading the data from flash, the DMA does not stall when accessing
the data. The DMA can then retrieve this data through the auxiliary AHB port, which provides direct single-cycle access
to the streaming data FIFO.
On RP2350, you can also use the auxiliary AHB port to access the QMI direct-mode FIFOs. This is faster than accessing
RP2350 Datasheet
4.4. External flash and PSRAM (XIP)
345


---
source_pdf: rp2350-datasheet-2.pdf
repository: llm_database
chapter: Chapter 4. Memory
section: 4.4.6. List of XIP_AUX registers
pages: 351-352
type: technical_spec
generated_at: 2026-02-28T17:43:10.557178+00:00
---

# 4.4.6. List of XIP_AUX registers

![Page 351 figure](images/fig_p0351.png)

RP2350 Datasheet

Description

FIFO stream address

| Bits | Description | Type | Reset |
| --- | --- | --- | --- |
| 31:2 | The address of the next word to be streamed from flash to the streaming
FIFO.
Increments automatically after each flash access.
Write the initial access address here before starting a streaming read. | RW | 0x00000000 |
| 1:0 | Reserved. | - | - |

Table 444.

STREAM_ADDR

Register

XIP: STREAM_CTR Register

Offset: 0x18

Description

FIFO stream control

| Bits | Description | Type | Reset |
| --- | --- | --- | --- |
| 31:22 | Reserved. | - | - |
| 21:0 | Write a nonzero value to start a streaming read. This will then
progress in the background, using flash idle cycles to transfer
a linear data block from flash to the streaming FIFO.
Decrements automatically (1 at a time) as the stream
progresses, and halts on reaching 0.
Write 0 to halt an in-progress stream, and discard any in-flight
read, so that a new stream can immediately be started (after
draining the FIFO and reinitialising STREAM_ADDR) | RW | 0x000000 |

Table 445.

XIP: STREAM_FIFO Register

Offset: 0x1c

Description

FIFO stream data

| Bits | Description | Type | Reset |
| --- | --- | --- | --- |
| 31:0 | Streamed data is buffered here, for retrieval by the system DMA.
This FIFO can also be accessed via the XIP_AUX slave, to avoid exposing
the DMA to bus stalls caused by other XIP traffic. | RF | 0x00000000 |

Table 446.

STREAM_FIFO

Register

4.4.6. List of XIP_AUX registers

The XIP_AUX port provides fast AHB access to the streaming FIFO and the QMI Direct Mode FIFOs, to reduce the cost of

DMA access to these FIFOs.

| Offset | Name | Info |
| --- | --- | --- |
| 0x0 | STREAM | Read the XIP stream FIFO (fast bus access to
XIP_CTRL_STREAM_FIFO) |
| 0x4 | QMI_DIRECT_TX | Write to the QMI direct-mode TX FIFO (fast bus access to
QMI_DIRECT_TX) |

Table 447. List of

4.4. External flash and PSRAM (XIP)
350

![Page 352 figure](images/fig_p0352.png)

RP2350 Datasheet

| Offset | Name | Info |
| --- | --- | --- |
| 0x8 | QMI_DIRECT_RX | Read from the QMI direct-mode RX FIFO (fast bus access to
QMI_DIRECT_RX) |

XIP_AUX: STREAM Register

Offset: 0x0

| Bits | Description | Type | Reset |
| --- | --- | --- | --- |
| 31:0 | Read the XIP stream FIFO (fast bus access to XIP_CTRL_STREAM_FIFO) | RF | 0x00000000 |

Table 448. STREAM

XIP_AUX: QMI_DIRECT_TX Register

Offset: 0x4

Description

Write to the QMI direct-mode TX FIFO (fast bus access to QMI_DIRECT_TX)

| Bits | Description | Type | Reset |
| --- | --- | --- | --- |
| 31:21 | Reserved. | - | - |
| 20 | NOPUSH: Inhibit the RX FIFO push that would correspond to this TX FIFO
entry.
Useful to avoid garbage appearing in the RX FIFO when pushing the command
at the beginning of a SPI transfer. | WF | 0x0 |
| 19 | OE: Output enable (active-high). For single width (SPI), this field is ignored, and
SD0 is always set to output, with SD1 always set to input.
For dual and quad width (DSPI/QSPI), this sets whether the relevant SDx pads
are set to output whilst transferring this FIFO record. In this case the
command/address should have OE set, and the data transfer should have OE
set or clear depending on the direction of the transfer. | WF | 0x0 |
| 18 | DWIDTH: Data width. If 0, hardware will transmit the 8 LSBs of the DIRECT_TX
DATA field, and return an 8-bit value in the 8 LSBs of DIRECT_RX. If 1, the full
16-bit width is used. 8-bit and 16-bit transfers can be mixed freely. | WF | 0x0 |
| 17:16 | IWIDTH: Configure whether this FIFO record is transferred with
single/dual/quad interface width (0/1/2). Different widths can be mixed freely. | WF | 0x0 |
|  | Enumerated values: |  |  |
|  | 0x0 → S: Single width |  |  |
|  | 0x1 → D: Dual width |  |  |
|  | 0x2 → Q: Quad width |  |  |
| 15:0 | DATA: Data pushed here will be clocked out falling edges of SCK (or before
the very first rising edge of SCK, if this is the first pulse). For each byte clocked
out, the interface will simultaneously sample one byte, on rising edges of SCK,
and push this to the DIRECT_RX FIFO.
For 16-bit data, the least-significant byte is transmitted first. | WF | 0x0000 |

Table 449.

QMI_DIRECT_TX

Register

XIP_AUX: QMI_DIRECT_RX Register

Offset: 0x8

4.4. External flash and PSRAM (XIP)
351

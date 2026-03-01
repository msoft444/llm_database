---
source_pdf: rp2350-datasheet-5.pdf
repository: llm_database
chapter: Chapter 12. Peripherals
section: 12.6.4. Data request (DREQ)
pages: 1101-1103
type: technical_spec
generated_at: 2026-03-01T04:13:03.328193+00:00
---

# 12.6.4. Data request (DREQ)

12.6.4. Data request (DREQ)

Peripherals produce or consume data at their own pace. If the DMA transferred data as fast as possible, loss or

corruption of data would ensue. DREQs are a communication channel between peripherals and the DMA that enables

the DMA to pace transfers according to the needs of the peripheral.

The CTRL.TREQ_SEL (transfer request) field selects an external DREQ. It can also be used to select one of the internal

pacing timers, or select no TREQ at all (the transfer proceeds as fast as possible), e.g. for memory-to-memory transfers.

12.6.4.1. System DREQ table

DREQ numbers use the following global assignment to peripheral DREQ channels:

12.6. DMA
1100

RP2350 Datasheet

| DREQ | DREQ Channel | DREQ | DREQ Channel | DREQ | DREQ Channel | DREQ | DREQ Channel |
| --- | --- | --- | --- | --- | --- | --- | --- |
| 0 | DREQ PIO0 TX0 _ _ | 14 | DREQ PIO1 RX2 _ _ | 28 | DREQ UART0 TX _ _ | 42 | DREQ PWM WRAP10 _ _ |
| 1 | DREQ PIO0 TX1 _ _ | 15 | DREQ PIO1 RX3 _ _ | 29 | DREQ UART0 RX _ _ | 43 | DREQ PWM WRAP11 _ _ |
| 2 | DREQ PIO0 TX2 _ _ | 16 | DREQ PIO2 TX0 _ _ | 30 | DREQ UART1 TX _ _ | 44 | DREQ I2C0 TX _ _ |
| 3 | DREQ PIO0 TX3 _ _ | 17 | DREQ PIO2 TX1 _ _ | 31 | DREQ UART1 RX _ _ | 45 | DREQ I2C0 RX _ _ |
| 4 | DREQ PIO0 RX0 _ _ | 18 | DREQ PIO2 TX2 _ _ | 32 | DREQ PWM WRAP0 _ _ | 46 | DREQ I2C1 TX _ _ |
| 5 | DREQ PIO0 RX1 _ _ | 19 | DREQ PIO2 TX3 _ _ | 33 | DREQ PWM WRAP1 _ _ | 47 | DREQ I2C1 RX _ _ |
| 6 | DREQ PIO0 RX2 _ _ | 20 | DREQ PIO2 RX0 _ _ | 34 | DREQ PWM WRAP2 _ _ | 48 | DREQ ADC _ |
| 7 | DREQ PIO0 RX3 _ _ | 21 | DREQ PIO2 RX1 _ _ | 35 | DREQ PWM WRAP3 _ _ | 49 | DREQ XIP STREAM _ _ |
| 8 | DREQ PIO1 TX0 _ _ | 22 | DREQ PIO2 RX2 _ _ | 36 | DREQ PWM WRAP4 _ _ | 50 | DREQ XIP QMITX _ _ |
| 9 | DREQ PIO1 TX1 _ _ | 23 | DREQ PIO2 RX3 _ _ | 37 | DREQ PWM WRAP5 _ _ | 51 | DREQ XIP QMIRX _ _ |
| 10 | DREQ PIO1 TX2 _ _ | 24 | DREQ SPI0 TX _ _ | 38 | DREQ PWM WRAP6 _ _ | 52 | DREQ HSTX _ |
| 11 | DREQ PIO1 TX3 _ _ | 25 | DREQ SPI0 RX _ _ | 39 | DREQ PWM WRAP7 _ _ | 53 | DREQ CORESIGHT _ |
| 12 | DREQ PIO1 RX0 _ _ | 26 | DREQ SPI1 TX _ _ | 40 | DREQ PWM WRAP8 _ _ | 54 | DREQ SHA256 _ |
| 13 | DREQ PIO1 RX1 _ _ | 27 | DREQ SPI1 RX _ _ | 41 | DREQ PWM WRAP9 _ _ |  |  |

12.6.4.2. Credit-based DREQ Scheme

The RP2350 DMA is designed for systems where:

• The area and power cost of large peripheral data FIFOs is prohibitive.
• The bandwidth demands of individual peripherals can be high, for example, >50% bus injection rate for short

periods.
• Bus latency is low, but multiple managers can compete for bus access.

In addition, the DMA’s transfer FIFOs and dual-manager-port structure permit multiple accesses to the same peripheral

to be in-flight at once to improve throughput. Choice of DREQ mechanism is therefore critical:

• The traditional "turn on the tap" method can cause overflow if multiple writes are backed up in the TDF. Some

systems solve this by over-provisioning peripheral FIFOs and setting the DREQ threshold below the full level at the

expense of precious area and power.
• The Arm-style single and burst handshake does not permit additional requests to be registered while the current

request is being served. This limits performance when FIFOs are very shallow.

The RP2350 DMA uses a credit-based DREQ mechanism. For each peripheral, the DMA attempts to keep as many

transfers in-flight as the peripheral has capacity for. This enables full bus throughput (1 word per clock) through an 8-

deep peripheral FIFO with no possibility of overflow or underflow in the absence of fabric latency or contention.

For each channel, the DMA maintains a counter. Each 1-clock pulse on the dreq signal increments this counter. When

non-zero, the channel requests a transfer from the DMA’s internal arbiter. The counter decrements when the transfer is

issued to the address FIFOs. At this point the transfer is in flight, but has not yet necessarily completed.

The counter is saturating, and six bits in size. The counter ignores increments at the maximum value or decrements at

zero. The six-bit counter size supports counts up to the depth of any FIFO on RP2350.

12.6. DMA
1101

RP2350 Datasheet

![Page 1103 figure](images/fig_p1103.png)

Figure 123. DREQ

counting

1
1
2
0
0

The effect is to upper bound the number of in-flight transfers based on the amount of room or data available in the

peripheral FIFO. In the steady state, this gives maximum throughput, but can’t underflow or underflow. This approach

has the following caveats:

• The user must not access a FIFO currently being serviced by the DMA. This causes the channel and peripheral to

become desynchronised, and can cause corruption or loss of data.
• Multiple channels must not be connected to the same DREQ.

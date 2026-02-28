---
source_pdf: rp2350-datasheet-1.pdf
repository: llm_database
chapter: Chapter 12. Peripherals
section: 12.2.11. Spike suppression
pages: 1002-1002
type: technical_spec
generated_at: 2026-02-28T17:22:56.644477+00:00
---

# 12.2.11. Spike suppression

12.2.10.3.1. Procedure
1. Define a timer interval (ti2c_poll) equal to the 10 times the signalling period for the highest I2C transfer speed used in
the system and supported by DW_apb_i2c. For example, if the highest I2C transfer mode is 400 kb/s, ti2c_poll is 25μs.
2. Define a maximum time-out parameter, MAX_T_POLL_COUNT, such that if any repeated polling operation exceeds this
maximum value, an error is reported.
3. Execute a blocking thread, process, or function that prevents any further I2C master transactions from starting
from software, but allows any pending transfers to be completed.
NOTE
This step can be ignored if DW_apb_i2c is programmed to operate as an I2C slave only.
1. The variable POLL_COUNT is initialized to zero.
2. Set bit zero of the IC_ENABLE register to zero.
3. Read the IC_ENABLE_STATUS register and test the IC_EN bit (bit 0). Increment POLL_COUNT by one. If
POLL_COUNT >= MAX_T_POLL_COUNT, exit with the relevant error code.
4. If IC_ENABLE_STATUS[0] is one, sleep for ti2c_poll and proceed to the previous step. Otherwise, exit with a
relevant success code.
12.2.10.4. Aborting I2C transfers
The ABORT control bit of the IC_ENABLE register allows the software to relinquish the I2C bus before completing the
issued transfer commands from the TX FIFO. In response to an ABORT request, the controller issues the STOP condition
over the I2C bus, followed by a TX FIFO flush. Aborting the transfer is allowed only in master mode of operation.
12.2.10.4.1. Procedure
1. Stop filling the TX FIFO (IC_DATA_CMD) with new commands.
2. When operating in DMA mode, disable the transmit DMA by setting TDMAE to zero.
3. Set IC_ENABLE.ABORT to one.
4. Wait for the M_TX_ABRT interrupt.
5. Read the IC_TX_ABRT_SOURCE register to identify the source as ABRT_USER_ABRT.
12.2.11. Spike suppression
The DW_apb_i2c contains programmable spike suppression logic that matches requirements imposed by the I2C Bus
Specification for SS/FS modes. This logic is based on counters that monitor the input signals (SCL and SDA), checking if
they remain stable for a predetermined amount of ic_clk cycles before they are sampled internally. There is one
separate counter for each signal (SCL and SDA). The number of ic_clk cycles can be programmed by the user. The value
should account for the frequency of ic_clk and the relevant spike length specification. Each counter starts whenever its
input signal changes value. Depending on the behaviour of the input signal, one of the following scenarios occurs:
• The input signal remains unchanged until the counter reaches its count limit value. When this happens, the counter
resets and stops, and the internal version of the signal updates to the input value.
• The input signal changes again before the counter reaches its count limit value. When this happens, the counter
resets and stops, but the internal version of the signal does not update.
The timing diagram in Figure 87 illustrates the behaviour described above.
RP2350 Datasheet
12.2. I2C
1001


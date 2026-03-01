---
source_pdf: rp2350-datasheet-8.pdf
repository: llm_database
chapter: Chapter 12. Peripherals
section: 12.2.10. Operation modes
pages: 997-1002
type: technical_spec
generated_at: 2026-03-01T08:45:43.944040+00:00
---

# 12.2.10. Operation modes

12.2.10. Operation modes

This section provides information about operation modes.

NOTE

Only set the DW_apb_i2c to operate as an I2C Master or an I2C Slave. Never set the DW_apb_i2c to operate as both

simultaneously. To avoid this, never simultaneously set IC_CON.IC_SLAVE_DISABLE and IC_CON.MASTER_MODE to

zero and one, respectively.

12.2.10.1. Slave mode operation

This section discusses slave mode procedures.

12.2.10.1.1. Initial configuration

To use the DW_apb_i2c as a slave, perform the following steps:

1. Disable the DW_apb_i2c by writing a 0 to IC_ENABLE.ENABLE.

2. Write to the IC_SAR register (bits 9:0) to set the slave address. This is the address to which the DW_apb_i2c

responds.

3. Write to the IC_CON register to specify which type of addressing is supported (7-bit or 10-bit by setting bit 3).

Enable the DW_apb_i2c in slave-only mode by writing a 0 into bit six (IC_CON.IC_SLAVE_DISABLE) and a 0 to bit zero

(IC_CON.MASTER_MODE).

12.2. I2C
996

RP2350 Datasheet

NOTE

![Page 998 figure](images/fig_p0998.png)

Slaves and masters can use different addressing settings. For instance, a slave can be programmed with 7-bit

addressing and a master with 10-bit addressing, and vice versa.

4. Enable the DW_apb_i2c by writing a 1 to IC_ENABLE.ENABLE.

Depending on the reset values chosen, steps two and three may not be necessary because the reset values can be

configured. For instance, if the device is only going to be a master, there would be no need to set the slave address

because you can configure DW_apb_i2c to have the slave disabled after reset and to enable the master after reset. The

values stored are static and do not need to be reprogrammed if the DW_apb_i2c is disabled.

Only bring the DW_apb_i2c Slave out of reset when the I2C bus is IDLE. De-asserting the reset when a transfer is

ongoing on the bus causes internal synchronization flip-flops used to synchronize SDA and SCL to toggle from a reset

value of one to the actual value on the bus. This can result in SDA toggling from one to zero while SCL is one, thereby

causing a false START condition to be detected by the DW_apb_i2c Slave. This scenario can also be avoided by

configuring the DW_apb_i2c with IC_SLAVE_DISABLE = 1 and MASTER_MODE = 1 so that the Slave interface is disabled after

reset. It can then be enabled by programming IC_CON[0] = 0 and IC_CON[6] = 0 after the internal SDA and SCL have

synchronized to the value on the bus; this takes approximately six ic_clk cycles after reset de-assertion.

12.2.10.1.2. Slave-transmitter operation for a single byte

When another I2C master device on the bus addresses the DW_apb_i2c and requests data, the DW_apb_i2c acts as a slave-

transmitter. The following steps occur:

1. The other I2C master device initiates an I2C transfer with an address that matches the slave address in the IC_SAR

2. The DW_apb_i2c acknowledges the sent address and recognizes the direction of the transfer to indicate that it is

acting as a slave-transmitter.

3. The DW_apb_i2c asserts the RD_REQ interrupt (bit five of the IC_RAW_INTR_STAT register) and holds the SCL line low. It

remains in a wait state until software responds. If the RD_REQ interrupt has been masked, due to

IC_INTR_MASK.M_RD_REQ being set to zero, use a hardware and/or software timing routine to instruct the CPU to

perform periodic reads of the IC_RAW_INTR_STAT register.

◦Reads that indicate IC_RAW_INTR_STAT.RD_REQ being set to one must be treated as the equivalent of the

RD_REQ interrupt being asserted.

◦Software must then act to satisfy the I2C transfer.

◦The timing interval used should be in the order of 10 times the fastest SCL clock period the DW_apb_i2c can

handle. For example, for 400 kb/s, the timing interval is 25μs.

The value of 10 is recommended here because this is approximately the amount of time required for a

single byte of data transferred on the I2C bus.

4. If there is any data remaining in the TX FIFO before receiving the read request, the DW_apb_i2c asserts a TX_ABRT

interrupt (bit six of the IC_RAW_INTR_STAT register) to flush the old data from the TX FIFO. If the TX_ABRT interrupt

has been masked, due to IC_INTR_MASK.M_TX_ABRT being set to zero, re-use the timing routine described in the

previous step to read the IC_RAW_INTR_STAT register.

12.2. I2C
997

RP2350 Datasheet

NOTE

Because the DW_apb_i2c's TX FIFO is forced into a flushed/reset state whenever a TX_ABRT event occurs, software

must release the DW_apb_i2c from this state by reading the IC_CLR_TX_ABRT register before attempting to write

into the TX FIFO. See register IC_RAW_INTR_STAT for more details.

◦Reads that indicate bit six (R_TX_ABRT) being set to one must be treated as the equivalent of the TX_ABRT

interrupt being asserted.

◦There is no further action required from software.

◦The timing interval used should be similar to that described in the previous step for the

IC_RAW_INTR_STAT.RD_REQ register.

5. Software writes to the IC_DATA_CMD register with the data to be written (by writing a 0 in bit 8).

6. Software must clear the RD_REQ and TX_ABRT interrupts (bits five and six, respectively) of the IC_RAW_INTR_STAT

register before proceeding. If the RD_REQ or TX_ABRT interrupts have been masked, then clearing of the

IC_RAW_INTR_STAT register will have already been performed when either the R_RD_REQ or R_TX_ABRT bit has been

read as one.

7. The DW_apb_i2c releases the SCL and transmits the byte.

8. The master may hold the I2C bus by issuing a RESTART condition or release the bus by issuing a STOP condition.

NOTE

Slave-Transmitter Operation for a single byte is not applicable in Ultra-Fast mode, since this mode does not support

read transfers.

12.2.10.1.3. Slave-receiver operation for a single byte

When another I2C master device on the bus addresses the DW_apb_i2c and is sending data, the DW_apb_i2c acts as a slave-

receiver and the following steps occur:

1. The other I2C master device initiates an I2C transfer with an address that matches the DW_apb_i2c's slave address in

the IC_SAR register.

2. The DW_apb_i2c acknowledges the sent address and recognizes the direction of the transfer to indicate that the

DW_apb_i2c is acting as a slave-receiver.

3. DW_apb_i2c receives the transmitted byte and places it in the receive buffer.

NOTE

If the Rx (receive) FIFO is completely filled with data when a byte is pushed, then the DW_apb_i2c slave holds the

I2C SCL line low until the Rx FIFO has some space, and then continues with the next read request.

4. DW_apb_i2c asserts the RX_FULL interrupt IC_RAW_INTR_STAT.RX_FULL. If the RX_FULL interrupt has been masked, due

to setting IC_INTR_MASK.M_RX_FULL to zero or setting IC_TX_TL to a value larger than zero, you should

implement a timing routine (described in Section 12.2.10.1.2) for periodic reads of the IC_STATUS register. This

timing routine should treat reads of the IC_STATUS register, with bit 3 (RFNE) set at one as the equivalent of an

RX_FULL interrupt.

5. Software may read the byte from the IC_DATA_CMD register (bits 7:0).

6. The other master device may hold the I2C bus by issuing a RESTART condition, or release the bus by issuing a

STOP condition.

12.2. I2C
998

RP2350 Datasheet

12.2.10.1.4. Slave-transfer operation for bulk transfers

In the standard I2C protocol, all transactions are single byte transactions; the programmer responds to a remote master

read request by writing one byte into the slave’s TX FIFO. When a slave (slave-transmitter) receives a read request

(RD_REQ) from the remote master (master-receiver), at a minimum there should be at least one entry placed into the

slave-transmitter’s TX FIFO.

DW_apb_i2c handles more data in the TX FIFO. This enables subsequent read requests to take data without raising an

interrupt. This eliminates latencies incurred between interrupts. This mode only occurs when DW_apb_i2c acts as a slave-

transmitter. If the remote master acknowledges the data sent by the slave-transmitter and there is no data in the slave’s

TX FIFO, the DW_apb_i2c holds the I2C SCL line low while it raises the read request interrupt (RD_REQ) and waits for a data

write into the TX FIFO.

If the RD_REQ interrupt is masked by setting IC_INTR_STAT.R_RD_REQ to zero, use a timing routine to activate periodic

reads of the IC_RAW_INTR_STAT register. Reads of IC_RAW_INTR_STAT that return bit five (RD_REQ) set to one must be

treated as the equivalent of RD_REQ. This timing routine is similar to that described in Section 12.2.10.1.2.

The RD_REQ interrupt is raised upon a read request. Always clear this interrupt when exiting the interrupt service handling

routine (ISR). The ISR allows you to either write one byte or more than one byte into the TX FIFO. The master can

request additional data at the end of a transmission by acknowledging the last byte. In this scenario, the slave must

raise RD_REQ again.

If you know in advance that the remote master requests a packet of n bytes, you can write n byte to the TX FIFO. Then,

when another master addresses DW_apb_i2c and requests data, the remote master will receive a continuous stream of

data. This happens because the DW_apb_i2c slave continues to send data to the remote master as long as the remote

master acknowledges the data sent and there is data available in the TX FIFO. There is no need to hold the SCL line low

or to issue RD_REQ again.

If the remote master doesn’t read all of the bytes from the TX FIFO, the DW_apb_i2c ignores the excess bytes with the

following procedure:

• The DW_apb_i2c clears the TX FIFO.
• The DW_apb_i2c generates a transmit abort (TX_ABRT) event.

At the time an ACK/NACK is expected, if a NACK is received, then the remote master has all the data it wants. At this

time, a flag is raised within the slave’s state machine to clear the leftover data in the TX FIFO. This flag is transferred to

the processor bus clock domain where the FIFO exists and the contents of the TX FIFO is cleared at that time.

12.2.10.2. Master mode operation

This section discusses master mode procedures.

12.2.10.2.1. Initial configuration

To use the DW_apb_i2c as a master, perform the following steps:

1. Disable the DW_apb_i2c by writing zero to IC_ENABLE.ENABLE.

2. Write to the IC_CON register to set the maximum speed mode supported (bits 2:1) and the desired speed of the

DW_apb_i2c master-initiated transfers, either 7-bit or 10-bit addressing (bit 4). Ensure that bit six

(IC_SLAVE_DISABLE) is written with a 1 and bit zero (MASTER_MODE) is written with a 1.

12.2. I2C
999

RP2350 Datasheet

NOTE

Slaves and masters can use different addressing settings. For instance, a slave can be programmed with 7-bit

addressing and a master with 10-bit addressing, and vice versa.

3. Write the address of the I2C device to be addressed to bits 9:0 of the IC_TAR register. This register also

determines whether the I2C will perform a General Call or a START BYTE command.

4. Enable the DW_apb_i2c by writing a one to IC_ENABLE.ENABLE.

5. Write the transfer direction and the data to be sent to the IC_DATA_CMD register. This step generates the START

condition and the address byte on the DW_apb_i2c. Once DW_apb_i2c is enabled and there is data in the TX FIFO,

DW_apb_i2c starts reading the data.

NOTE

If you write to the IC_DATA_CMD register before enabling the DW_apb_i2c, the data and commands are lost: the

buffers are kept cleared when DW_apb_i2c is disabled.

The values stored are static and do not need to be reprogrammed when the DW_apb_i2c is disabled except for transfer

direction and data. As a result, you may not need to perform steps two, three, four, and five if you already configured the

reset values.

12.2.10.2.2. Master transmit and master receive

The DW_apb_i2c supports switching back and forth between reading and writing dynamically. To transmit data, write data

to the lower byte of the I2C RX/TX Data Buffer and Command Register (IC_DATA_CMD). For I2C write operations, write

zero to the CMD bit [8]. Subsequently, to issue a read command, write a one to the CMD bit and write don’t care to the

lower byte of the IC_DATA_CMD register. The DW_apb_i2c master continues to initiate transfers as long as there are

commands present in the TX FIFO. If the TX FIFO becomes empty, the master performs one of the following actions

based on the value of IC_DATA_CMD:

• If set to one, it issues a STOP condition after completing the current transfer.
• If set to zero, it holds SCL low until next command is written to the TX FIFO.

For more details, refer to Section 12.2.7.

12.2.10.3. Disabling DW_apb_i2c

The IC_ENABLE_STATUS register allows software to unambiguously determine when the I2C hardware has completely

shut down.

NOTE

Earlier versions of 
DW_apb_i2c required the programmer to monitor two registers: (IC_STATUS and

IC_RAW_INTR_STAT). RP2350 only requires the programmer to monitor IC_ENABLE_STATUS.

To shut down I2C hardware, write a zero to IC_ENABLE.ENABLE. The DW_apb_i2c master can be disabled only if the

command currently processing when the de-assertion occurs has the STOP bit set to one. If you attempt to disable the

DW_apb_i2c master while processing a command without the STOP bit set, the DW_apb_i2c master continues to remain

active, holding the SCL line low until a new command is received in the TX FIFO.

To relinquish the I2C bus and disable DW_apb_i2c while the DW_apb_i2c master is processing a command without the STOP

bit set, issue an ABORT request.

12.2. I2C
1000

RP2350 Datasheet

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

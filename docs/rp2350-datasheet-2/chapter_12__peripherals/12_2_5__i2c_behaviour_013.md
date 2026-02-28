---
source_pdf: rp2350-datasheet-2.pdf
repository: llm_database
chapter: Chapter 12. Peripherals
section: 12.2.5. I2C behaviour
pages: 989-989
type: technical_spec
generated_at: 2026-02-28T17:43:10.557178+00:00
---

# 12.2.5. I2C behaviour

![Page 989 figure](images/fig_p0989.png)

RP2350 Datasheet

12.2.4.2. Bus transfer terms

The following terms are specific to data transfers that occur to and from the I2C bus.

START (RESTART)

data transfer begins with a START or RESTART condition. The level of the SDA data line changes from high to low,

while the SCL clock line remains high. When this occurs, the bus becomes busy.

NOTE

START and RESTART conditions are functionally identical.

STOP

data transfer is terminated by a STOP condition. This occurs when the level on the SDA data line passes from the low

state to the high state, while the SCL clock line remains high. When the data transfer has been terminated, the bus is

free or idle once again. The bus stays busy if a RESTART is generated instead of a STOP condition.

12.2.5. I2C behaviour

The DW_apb_i2c can be controlled with software to be one of the following:

• An I2C master only, communicating with other I2C slaves
• An I2C slave only, communicating with one or more I2C masters.

The master is responsible for generating the clock and controlling the transfer of data. The slave is responsible for

either transmitting or receiving data to and from the master. The acknowledgement of data is sent by the device that is

receiving data, which can be either a master or a slave. As mentioned previously, the I2C protocol also allows multiple

masters to reside on the I2C bus and uses an arbitration procedure to determine bus ownership.

Each slave has a unique address determined by the system designer. When a master wants to communicate with a

slave:

1. The master transmits a START/RESTART condition that is then followed by the slave’s address and a control bit

(R/W) to determine if the master wants to transmit data or receive data from the slave.

2. The slave then sends an acknowledge (ACK) pulse after the address.

When the master (master-transmitter) writes to the slave (slave-receiver), the receiver gets one byte of data. This

transaction continues until the master terminates the transmission with a STOP condition.

When the master reads from a slave (master-receiver), the slave transmits (slave-transmitter) a byte of data to the

master. The master then acknowledges the transaction with the ACK pulse. This transaction continues until the master

terminates the transmission by not acknowledging (NACK) the transaction after the last byte is received, and then the

master issues a STOP condition or addresses another slave after issuing a RESTART condition. This behaviour is

illustrated in Figure 69.

Figure 69. Data

|  |  |  |  |  | P
or
R |  |
| --- | --- | --- | --- | --- | --- | --- |
|  |  |  | ACK |  |  |  |
|  |  |  |  |  |  |  |
|  |  |  |  |  | R
or
P |  |

transfer on the I2C

MSB |  |  |  | LSB

LSB
ACK

SDA

Bus

1
2
1
2
9
3-8
7
8
9

SCL
S
or
R

SCL held low while 
servicing interrupts

START or RESTART Condition

STOP AND RESTART Condition
Byte Complete Interrupt 
within Slave

The DW_apb_i2c is a synchronous serial interface. The SDA line is a bidirectional signal that changes only while the SCL line

is low except for STOP, START, and RESTART conditions. The output drivers are open-drain or open-collector to perform

wire-AND functions on the bus. The maximum number of devices on the bus is limited by only the maximum

capacitance specification of 400 pF. Data is transmitted in byte packages.

The I2C protocols implemented in DW_apb_i2c are described in more details in Section 12.2.6.

12.2. I2C
988

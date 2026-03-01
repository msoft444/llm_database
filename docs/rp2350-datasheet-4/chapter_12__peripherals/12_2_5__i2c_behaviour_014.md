---
source_pdf: rp2350-datasheet-4.pdf
repository: llm_database
chapter: Chapter 12. Peripherals
section: 12.2.5. I2C behaviour
pages: 989-989
type: technical_spec
generated_at: 2026-03-01T03:27:49.792955+00:00
---

# 12.2.5. I2C behaviour

![Page 989 figure](images/fig_p0989.png)

The DW_apb_i2c can be controlled with software to be one of the following:

• An I2C master only, communicating with other I2C slaves
• An I2C slave only, communicating with one or more I2C masters.

The master is responsible for generating the clock and controlling the transfer of data. The slave is responsible for

either transmitting or receiving data to and from the master. The acknowledgement of data is sent by the device that is

receiving data, which can be either a master or a slave. As mentioned previously, the I2C protocol also allows multiple

masters to reside on the I2C bus and uses an arbitration procedure to determine bus ownership.

Each slave has a unique address determined by the system designer. When a master wants to communicate with a

1. The master transmits a START/RESTART condition that is then followed by the slave’s address and a control bit

(R/W) to determine if the master wants to transmit data or receive data from the slave.

2. The slave then sends an acknowledge (ACK) pulse after the address.

When the master (master-transmitter) writes to the slave (slave-receiver), the receiver gets one byte of data. This

transaction continues until the master terminates the transmission with a STOP condition.

When the master reads from a slave (master-receiver), the slave transmits (slave-transmitter) a byte of data to the

master. The master then acknowledges the transaction with the ACK pulse. This transaction continues until the master

terminates the transmission by not acknowledging (NACK) the transaction after the last byte is received, and then the

master issues a STOP condition or addresses another slave after issuing a RESTART condition. This behaviour is

Figure 69. Data

transfer on the I2C

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

The DW_apb_i2c is a synchronous serial interface. The SDA line is a bidirectional signal that changes only while the SCL line

is low except for STOP, START, and RESTART conditions. The output drivers are open-drain or open-collector to perform

wire-AND functions on the bus. The maximum number of devices on the bus is limited by only the maximum

capacitance specification of 400 pF. Data is transmitted in byte packages.

The I2C protocols implemented in DW_apb_i2c are described in more details in Section 12.2.6.

12.2. I2C
988

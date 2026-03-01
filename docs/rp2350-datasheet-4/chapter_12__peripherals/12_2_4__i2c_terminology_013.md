---
source_pdf: rp2350-datasheet-4.pdf
repository: llm_database
chapter: Chapter 12. Peripherals
section: 12.2.4. I2C terminology
pages: 988-988
type: technical_spec
generated_at: 2026-03-01T03:27:49.792955+00:00
---

# 12.2.4. I2C terminology

12.2.4. I2C terminology

This section defines key terms used in various parts of the I2C.

12.2.4.1. I2C bus terms

The following terms relate to how the role of the I2C device and how it interacts with other I2C devices on the bus.

Transmitter

the device that sends data to the bus. A transmitter can either be a device that initiates the data transmission to the

bus (a master-transmitter) or the device that responds to a request from the master to send data to the bus (a

slave-transmitter).

Receiver

the device that receives data from the bus. A receiver can either be a device that receives data on its own request (a

master-receiver) or a device that receives data in response to a request from the master (a slave-receiver).

Master

the component that initializes a transfer (START command), generates the clock SCL signal and terminates the

transfer (STOP command). A master can be either a transmitter or a receiver.

Slave

the device addressed by the master. A slave can be either receiver or transmitter.

Multi-master

the ability for more than one master to co-exist on the bus at the same time without collision or data loss.

Arbitration

the predefined procedure that authorizes only one master at a time to take control of the bus. For more information

about this behaviour, refer to Section 12.2.8.

Synchronization

the predefined procedure that synchronizes the clock signals provided by two or more masters. For more

information about this feature, refer to Section 12.2.9.

SDA

the data signal line (Serial Data).

SCL

the clock signal line (Serial Clock).

12.2. I2C
987

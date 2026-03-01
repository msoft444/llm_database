---
source_pdf: rp2350-datasheet-3.pdf
repository: llm_database
chapter: Chapter 12. Peripherals
section: 12.2.3. I2C overview
pages: 986-987
type: technical_spec
generated_at: 2026-03-01T02:30:35.833805+00:00
---

# 12.2.3. I2C overview

![Page 986 figure](images/fig_p0986.png)

RP2350 Datasheet

• 10-bit addressing supported in master mode (7-bit by default)
• 16 entry transmit buffer
• 16 entry receive buffer
• Allows restart conditions when a master (can be disabled for legacy device support)
• Configurable timing to adjust TsuDAT/ThDAT
• General calls responded to on reset
• Interface to DMA
• Single interrupt output
• Configurable timing to adjust clock frequency
• Spike suppression (default 7 clk_sys cycles)
• Can NACK after data received by Slave
• Hold transfer when TX FIFO empty
• Hold bus until space available in RX FIFO
• Restart detect interrupt in Slave mode
• Optional blocking Master commands (not enabled by default)

12.2.3. I2C overview

The I2C bus is a 2-wire serial interface, consisting of a serial data line SDA and a serial clock SCL. These wires carry

information between the devices connected to the bus. Each device is recognized by a unique address and can operate

as either a "transmitter" or "receiver", depending on the function of the device. Devices can also be considered as

masters or slaves when performing data transfers. A master is a device that initiates a data transfer on the bus and

generates the clock signals to permit that transfer. At that time, any device addressed is considered a slave.

NOTE

The I2C block must only be programmed to operate in either master OR slave mode only. Operating as a master and

slave simultaneously is not supported.

The I2C block can operate in these modes:

• standard mode (with data rates from 0 to 100 kb/s),
• fast mode (with data rates up to 400 kb/s),
• fast mode plus (with data rates up to 1000 kb/s).

These modes are not supported:

• High-speed mode (with data rates up to 3.4Mb/s),
• Ultra-Fast Speed Mode (with data rates up to 5Mb/s).

NOTE

References to fast mode also apply to fast mode plus, unless specifically stated otherwise.

The I2C block can communicate with devices in one of these modes as long as they are attached to the bus.

Additionally, fast mode devices are downward compatible. For instance, fast mode devices can communicate with

standard mode devices at up to 100 kb/s over the I2C bus system. However, standard mode devices are not upward

compatible and should not be incorporated in a fast-mode I2C bus system as they cannot follow the higher transfer

rate; unpredictable states would occur.

12.2. I2C
985

![Page 987 figure](images/fig_p0987.png)

RP2350 Datasheet

The following devices commonly use high-speed mode:

• LCD displays
• high-bit count ADCs
• high capacity EEPROMs

These devices typically need to transfer large amounts of data.

Most maintenance and control applications, the common use for the I2C bus, typically operate at 100 kHz in standard

and fast modes. Any DW_apb_i2c device can be attached to an I2C bus. Every device can talk with any master, passing

information back and forth. There needs to be at least one master (such as a microcontroller or DSP) on the bus, but

there can be multiple masters, which require them to arbitrate for ownership. Multiple masters and arbitration are

explained later in this chapter. The I2C block does not support SMBus and PMBus protocols (for System management

and Power management).

The DW_apb_i2c is made up of:

• an AMBA APB slave interface
• an I2C interface
• FIFO logic to maintain coherency between the two interfaces

The blocks of the component are illustrated in Figure 68.

Figure 68. I2C Block

diagram

DW_apb_i2c

AMBA Bus 
Interface Unit
Register File
Slave State 

Master State 

Machine

Machine

Clock Generator
Rx Shift
Tx Shift
Rx Filter

Toggle
Synchronizer
DMA Interface
Interrupt 
Controller

RX FIFO
TX FIFO

The following define the functions of the blocks in Figure 68:

• AMBA Bus Interface Unit: Takes the APB interface signals and translates them into a common generic interface

that allows the register file to be bus protocol-agnostic.
• Register File: Contains configuration registers and is the interface with software.
• Slave State Machine: Follows the protocol for a slave and monitors bus for address match.
• Master State Machine: Generates the I2C protocol for the master transfers.
• Clock Generator: Calculates the required timing to do the following:

◦Generate the SCL clock when configured as a master

◦Check for bus idle

◦Generate a START and a STOP

◦Setup the data and hold the data
• RX Shift: Takes data into the design and extracts it in byte format.

12.2. I2C
986

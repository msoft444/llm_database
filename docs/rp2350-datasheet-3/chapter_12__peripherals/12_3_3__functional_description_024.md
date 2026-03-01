---
source_pdf: rp2350-datasheet-3.pdf
repository: llm_database
chapter: Chapter 12. Peripherals
section: 12.3.3. Functional description
pages: 1048-1050
type: technical_spec
generated_at: 2026-03-01T02:30:35.833805+00:00
---

# 12.3.3. Functional description

RP2350 Datasheet

12.3.1. Changes from RP2040

The output enable of the SSPTXD data output (connecting to pins listed as SPI0 TX and SPI1 TX in the GPIO function

tables) is controlled by the SPI peripheral nSSPOE signal. The peripheral automatically tristates its output when

deselected in slave mode. This makes software control of the output enable unnecessary even when multiple slaves

share the data lines.

12.3.2. Overview

The PrimeCell SSP is a master or slave interface for synchronous serial communication with peripheral devices that

have Motorola SPI, National Semiconductor Microwire, or Texas Instruments synchronous serial interfaces.

The PrimeCell SSP performs serial-to-parallel conversion on data received from a peripheral device. The CPU accesses

data, control, and status information through the AMBA APB interface. The transmit and receive paths are buffered with

internal FIFO memories, enabling up to eight 16-bit values to be stored independently in both transmit and receive

modes. Serial data transmits on SSPTXD and is received on SSPRXD.

The PrimeCell SSP includes a programmable bit rate clock divider and prescaler to generate the serial output clock,

SSPCLKOUT, from the input clock, SSPCLK. Bit rates are supported to 2MHz and higher, subject to choice of frequency for

SSPCLK, and the maximum bit rate is determined by peripheral devices.

You can use the control registers SSPCR0 and SSPCR1 to program the PrimeCell SSP operating mode, frame format, and

size.

The following individually maskable interrupts are generated:

• SSPTXINTR requests servicing of the transmit buffer
• SSPRXINTR requests servicing of the receive buffer
• SSPRORINTR indicates an overrun condition in the receive FIFO
• SSPRTINTR indicates that a timeout period expired while data was present in the receive FIFO.

A single combined interrupt is asserted if any of the individual interrupts are asserted and unmasked. This interrupt is

connected to the processor interrupt controllers in RP2350.

In addition to the above interrupts, a set of DMA signals are provided for interfacing with a DMA controller.

Depending on the operating mode selected, the SSPFSSOUT output operates as:

• an active-HIGH frame synchronization output for Texas Instruments synchronous serial frame format
• an active-LOW slave select for SPI and Microwire.

12.3.3. Functional description

12.3. SPI
1047

![Page 1049 figure](images/fig_p1049.png)

RP2350 Datasheet

Figure 91. PrimeCell

SSPTXINTR

SSP block diagram.

For clarity, does not

PRESETn

PWDATAIn[15:0]
SSPTXINTR

Tx FIFO 
16 bits wide, 

show the test logic.

PSEL

PCLK

8 locations 

TxRdDataIn[15:0]

PENABLE

deep

PWRITE

AMBA 

FIFO status 
and interrupt 

SSPINTR

APB 
interface

PADDR[11:2]

generation

RxFRdData 
[15:0]

PWDATA[15:0]

SSPRXINTR
SSPRORINTR
SSPRTINTR

PRDATA[15:0]

Rx FIFO 
16 bits wide, 

PCLK

PCLK

8 locations 

PCLK

deep

SSPRTRINTR

SSPRORINTR

DATAOUT
DATAIN

SSPRXRINTR

SSPCLK

SSPCLK

nSSPOE

PCLK

SSPCLK

Tx/Rx params

Clock 
prescaler
Register 

nSSPRST

SSPCLKDIV

Prescale value

SSPTXD

block

SSPFSSOUT

Transmit and 

SSPRXDMACLR

receive logic

SSPCLKOUT

SSPTXDMACLR

Tx/Rx FIFO watermark levels

nSSPCTLOE

SSPRXDMASREQ

SSPCLKIN

DMA 
interface

SSPRXDMABREQ

SSPFSSIN

SSPTXDMASREQ

SSPRXD

RxWrData[15:0]

SSPTXDMABREQ

12.3.3.1. AMBA APB interface

The AMBA APB interface generates read and write decodes for accesses to status and control registers, and transmit

and receive FIFO memories.

12.3.3.2. Register block

The register block stores data written, or to be read, across the AMBA APB interface.

12.3.3.3. Clock prescaler

When configured as a master, an internal prescaler, comprising two free-running reloadable serially linked counters,

provides the serial output clock SSPCLKOUT.

You can program the clock prescaler, using the SSPCPSR register, to divide SSPCLK by a factor of 2-254 in steps of two. By

not utilizing the least significant bit of the SSPCPSR register, division by an odd number is not possible; this ensures that a

symmetrical clock with equal mark-space ratio is generated. See SSPCPSR.

The output of the prescaler is divided again by a factor of 1-256, by programming the SSPCR0 control register, to give

the final master output clock SSPCLKOUT.

NOTE

The PCLK and SSPCLK clock inputs in Figure 91 are connected to the clk_sys and clk_peri system-level clock nets on

RP2350, respectively. By default, clk_peri attaches directly to the system clock. However, you can detach it to

maintain constant SPI frequency if the system clock is varied dynamically. See Figure 33 for an overview of the

RP2350 clock architecture.

12.3.3.4. Transmit FIFO

The common transmit (TX) FIFO is a 16-bit wide, 8-location deep memory buffer. CPU data written across the AMBA

12.3. SPI
1048

RP2350 Datasheet

APB interface is stored in the buffer until read out by the transmit logic.

When configured as a master or a slave, parallel data is written into the transmit FIFO prior to serial conversion, and

transmission to the attached slave or master respectively, through the SSPTXD pin.

12.3.3.5. Receive FIFO

The common receive (RX) FIFO is a 16-bit wide, 8-location deep memory buffer. Received data from the serial interface

is stored in the buffer until read out by the CPU across the AMBA APB interface.

When configured as a master or slave, serial data received through the SSPRXD pin is registered prior to parallel loading

into the attached slave or master receive FIFO respectively.

12.3.3.6. Transmit and receive logic

When configured as a master, the clock for the attached slaves is derived from a divided-down version of SSPCLK through

the previously described prescaler operations. The master transmit logic successively reads a value from its transmit

FIFO and performs parallel to serial conversion on it. Then, the serial data stream and frame control signal,

synchronized to SSPCLKOUT, outputs through the SSPTXD pin to the attached slaves. The master receive logic performs

serial to parallel conversion on the incoming synchronous SSPRXD data stream, extracting and storing values into its

receive FIFO for subsequent reading through the APB interface.

When configured as a slave, the SSPCLKIN clock is provided by an attached master and used to time transmission and

reception sequences. The slave transmit logic, under control of the master clock, successively:

1. Reads a value from its transmit FIFO.

2. Performs parallel to serial conversion.

3. Outputs the serial data stream and frame control signal through the slave SSPTXD pin.

The slave receive logic performs serial to parallel conversion on the incoming SSPRXD data stream, extracting and storing

values into its receive FIFO, for subsequent reading through the APB interface.

12.3.3.7. Interrupt generation logic

The PrimeCell SSP generates four individual maskable, active-HIGH interrupts. A combined interrupt output is generated

as an OR function of the individual interrupt requests.

The transmit and receive dynamic data-flow interrupts, SSPTXINTR and SSPRXINTR, are separated from the status interrupts

so that data can be read or written in response to the FIFO trigger levels.

12.3.3.8. DMA interface

The PrimeCell SSP provides an interface to connect to a DMA controller, see Section 12.3.4.16.

12.3.3.9. Synchronizing registers and logic

The PrimeCell SSP supports both asynchronous and synchronous operation of the clocks, PCLK and SSPCLK.

Synchronization registers and handshaking logic have been implemented, and are active at all times. Synchronization of

control signals is performed on both directions of data flow, that is:

• from the PCLK to the SSPCLK domain
• from the SSPCLK to the PCLK domain.

12.3. SPI
1049

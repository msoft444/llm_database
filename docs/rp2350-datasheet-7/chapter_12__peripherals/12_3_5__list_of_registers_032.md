---
source_pdf: rp2350-datasheet-7.pdf
repository: llm_database
chapter: Chapter 12. Peripherals
section: 12.3.5. List of registers
pages: 1061-1067
type: technical_spec
generated_at: 2026-03-01T06:07:53.318745+00:00
---

# 12.3.5. List of registers

12.3.5. List of registers

The SPI0 and SPI1 registers start at base addresses of 0x40080000 and 0x40088000 respectively (defined as SPI0_BASE and

SPI1_BASE in SDK).

| Offset | Name | Info |
| --- | --- | --- |
| 0x000 | SSPCR0 | Control register 0, SSPCR0 on page 3-4 |
| 0x004 | SSPCR1 | Control register 1, SSPCR1 on page 3-5 |
| 0x008 | SSPDR | Data register, SSPDR on page 3-6 |
| 0x00c | SSPSR | Status register, SSPSR on page 3-7 |
| 0x010 | SSPCPSR | Clock prescale register, SSPCPSR on page 3-8 |
| 0x014 | SSPIMSC | Interrupt mask set or clear register, SSPIMSC on page 3-9 |
| 0x018 | SSPRIS | Raw interrupt status register, SSPRIS on page 3-10 |
| 0x01c | SSPMIS | Masked interrupt status register, SSPMIS on page 3-11 |
| 0x020 | SSPICR | Interrupt clear register, SSPICR on page 3-11 |
| 0x024 | SSPDMACR | DMA control register, SSPDMACR on page 3-12 |
| 0xfe0 | SSPPERIPHID0 | Peripheral identification registers, SSPPeriphID0-3 on page 3-13 |
| 0xfe4 | SSPPERIPHID1 | Peripheral identification registers, SSPPeriphID0-3 on page 3-13 |
| 0xfe8 | SSPPERIPHID2 | Peripheral identification registers, SSPPeriphID0-3 on page 3-13 |
| 0xfec | SSPPERIPHID3 | Peripheral identification registers, SSPPeriphID0-3 on page 3-13 |
| 0xff0 | SSPPCELLID0 | PrimeCell identification registers, SSPPCellID0-3 on page 3-16 |
| 0xff4 | SSPPCELLID1 | PrimeCell identification registers, SSPPCellID0-3 on page 3-16 |
| 0xff8 | SSPPCELLID2 | PrimeCell identification registers, SSPPCellID0-3 on page 3-16 |
| 0xffc | SSPPCELLID3 | PrimeCell identification registers, SSPPCellID0-3 on page 3-16 |

Table 1099. List of SPI

12.3. SPI
1060

RP2350 Datasheet

SPI: SSPCR0 Register

Offset: 0x000

Description

Control register 0, SSPCR0 on page 3-4

| Bits | Description | Type | Reset |
| --- | --- | --- | --- |
| 31:16 | Reserved. | - | - |
| 15:8 | SCR: Serial clock rate. The value SCR is used to generate the transmit and receive bit rate of the PrimeCell SSP. The bit rate is: F SSPCLK CPSDVSR x (1+SCR) where CPSDVSR is an even value from 2-254, programmed through the SSPCPSR register and SCR is a value from 0-255. | RW | 0x00 |
| 7 | SPH: SSPCLKOUT phase, applicable to Motorola SPI frame format only. See Motorola SPI frame format on page 2-10. | RW | 0x0 |
| 6 | SPO: SSPCLKOUT polarity, applicable to Motorola SPI frame format only. See Motorola SPI frame format on page 2-10. | RW | 0x0 |
| 5:4 | FRF: Frame format: 00 Motorola SPI frame format. 01 TI synchronous serial frame format. 10 National Microwire frame format. 11 Reserved, undefined operation. | RW | 0x0 |
| 3:0 | DSS: Data Size Select: 0000 Reserved, undefined operation. 0001 Reserved, undefined operation. 0010 Reserved, undefined operation. 0011 4-bit data. 0100 5-bit data. 0101 6-bit data. 0110 7-bit data. 0111 8-bit data. 1000 9-bit data. 1001 10-bit data. 1010 11-bit data. 1011 12-bit data. 1100 13-bit data. 1101 14-bit data. 1110 15-bit data. 1111 16-bit data. | RW | 0x0 |
| 31:4 | Reserved. | - | - |
| 3 | SOD: Slave-mode output disable. This bit is relevant only in the slave mode, MS=1. In multiple-slave systems, it is possible for an PrimeCell SSP master to broadcast a message to all slaves in the system while ensuring that only one slave drives data onto its serial output line. In such systems the RXD lines from multiple slaves could be tied together. To operate in such systems, the SOD bit can be set if the PrimeCell SSP slave is not supposed to drive the SSPTXD line: 0 SSP can drive the SSPTXD output in slave mode. 1 SSP must not drive the SSPTXD output in slave mode. | RW | 0x0 |
| 2 | MS: Master or slave mode select. This bit can be modified only when the PrimeCell SSP is disabled, SSE=0: 0 Device configured as master, default. 1 Device configured as slave. | RW | 0x0 |
| 1 | SSE: Synchronous serial port enable: 0 SSP operation disabled. 1 SSP operation enabled. | RW | 0x0 |
| 0 | LBM: Loop back mode: 0 Normal serial port operation enabled. 1 Output of transmit serial shifter is connected to input of receive serial shifter internally. | RW | 0x0 |

Table 1100. SSPCR0

SPI: SSPCR1 Register

Offset: 0x004

Description

Control register 1, SSPCR1 on page 3-5

12.3. SPI
1061

RP2350 Datasheet

Table 1101. SSPCR1

SPI: SSPDR Register

Offset: 0x008

Description

Data register, SSPDR on page 3-6

| Bits | Description | Type | Reset |
| --- | --- | --- | --- |
| 31:16 | Reserved. | - | - |
| 15:0 | DATA: Transmit/Receive FIFO: Read Receive FIFO. Write Transmit FIFO. You must right-justify data when the PrimeCell SSP is programmed for a data size that is less than 16 bits. Unused bits at the top are ignored by transmit logic. The receive logic automatically right-justifies. | RWF | - |

Table 1102. SSPDR

SPI: SSPSR Register

Offset: 0x00c

Description

Status register, SSPSR on page 3-7

| Bits | Description | Type | Reset |
| --- | --- | --- | --- |
| 31:5 | Reserved. | - | - |
| 4 | BSY: PrimeCell SSP busy flag, RO: 0 SSP is idle. 1 SSP is currently transmitting and/or receiving a frame or the transmit FIFO is not empty. | RO | 0x0 |
| 3 | RFF: Receive FIFO full, RO: 0 Receive FIFO is not full. 1 Receive FIFO is full. | RO | 0x0 |
| 2 | RNE: Receive FIFO not empty, RO: 0 Receive FIFO is empty. 1 Receive FIFO is not empty. | RO | 0x0 |
| 1 | TNF: Transmit FIFO not full, RO: 0 Transmit FIFO is full. 1 Transmit FIFO is not full. | RO | 0x1 |
| 0 | TFE: Transmit FIFO empty, RO: 0 Transmit FIFO is not empty. 1 Transmit FIFO is empty. | RO | 0x1 |
| 31:8 | Reserved. | - | - |
| 7:0 | CPSDVSR: Clock prescale divisor. Must be an even number from 2-254, depending on the frequency of SSPCLK. The least significant bit always returns zero on reads. | RW | 0x00 |

Table 1103. SSPSR

12.3. SPI
1062

RP2350 Datasheet

SPI: SSPCPSR Register

Offset: 0x010

Description

Clock prescale register, SSPCPSR on page 3-8

Table 1104. SSPCPSR

SPI: SSPIMSC Register

Offset: 0x014

Description

Interrupt mask set or clear register, SSPIMSC on page 3-9

| Bits | Description | Type | Reset |
| --- | --- | --- | --- |
| 31:4 | Reserved. | - | - |
| 3 | TXIM: Transmit FIFO interrupt mask: 0 Transmit FIFO half empty or less condition interrupt is masked. 1 Transmit FIFO half empty or less condition interrupt is not masked. | RW | 0x0 |
| 2 | RXIM: Receive FIFO interrupt mask: 0 Receive FIFO half full or less condition interrupt is masked. 1 Receive FIFO half full or less condition interrupt is not masked. | RW | 0x0 |
| 1 | RTIM: Receive timeout interrupt mask: 0 Receive FIFO not empty and no read prior to timeout period interrupt is masked. 1 Receive FIFO not empty and no read prior to timeout period interrupt is not masked. | RW | 0x0 |
| 0 | RORIM: Receive overrun interrupt mask: 0 Receive FIFO written to while full condition interrupt is masked. 1 Receive FIFO written to while full condition interrupt is not masked. | RW | 0x0 |

Table 1105. SSPIMSC

SPI: SSPRIS Register

Offset: 0x018

Description

Raw interrupt status register, SSPRIS on page 3-10

| Bits | Description | Type | Reset |
| --- | --- | --- | --- |
| 31:4 | Reserved. | - | - |
| 3 | TXRIS: Gives the raw interrupt state, prior to masking, of the SSPTXINTR interrupt | RO | 0x1 |
| 2 | RXRIS: Gives the raw interrupt state, prior to masking, of the SSPRXINTR interrupt | RO | 0x0 |
| 1 | RTRIS: Gives the raw interrupt state, prior to masking, of the SSPRTINTR interrupt | RO | 0x0 |
| 0 | RORRIS: Gives the raw interrupt state, prior to masking, of the SSPRORINTR interrupt | RO | 0x0 |

Table 1106. SSPRIS

12.3. SPI
1063

RP2350 Datasheet

SPI: SSPMIS Register

Offset: 0x01c

Description

Masked interrupt status register, SSPMIS on page 3-11

| Bits | Description | Type | Reset |
| --- | --- | --- | --- |
| 31:4 | Reserved. | - | - |
| 3 | TXMIS: Gives the transmit FIFO masked interrupt state, after masking, of the SSPTXINTR interrupt | RO | 0x0 |
| 2 | RXMIS: Gives the receive FIFO masked interrupt state, after masking, of the SSPRXINTR interrupt | RO | 0x0 |
| 1 | RTMIS: Gives the receive timeout masked interrupt state, after masking, of the SSPRTINTR interrupt | RO | 0x0 |
| 0 | RORMIS: Gives the receive over run masked interrupt status, after masking, of the SSPRORINTR interrupt | RO | 0x0 |

Table 1107. SSPMIS

SPI: SSPICR Register

Offset: 0x020

Description

Interrupt clear register, SSPICR on page 3-11

| Bits | Description | Type | Reset |
| --- | --- | --- | --- |
| 31:2 | Reserved. | - | - |
| 1 | RTIC: Clears the SSPRTINTR interrupt | WC | 0x0 |
| 0 | RORIC: Clears the SSPRORINTR interrupt | WC | 0x0 |

Table 1108. SSPICR

SPI: SSPDMACR Register

Offset: 0x024

Description

DMA control register, SSPDMACR on page 3-12

| Bits | Description | Type | Reset |
| --- | --- | --- | --- |
| 31:2 | Reserved. | - | - |
| 1 | TXDMAE: Transmit DMA Enable. If this bit is set to 1, DMA for the transmit FIFO is enabled. | RW | 0x0 |
| 0 | RXDMAE: Receive DMA Enable. If this bit is set to 1, DMA for the receive FIFO is enabled. | RW | 0x0 |
| 31:8 | Reserved. | - | - |
| 7:0 | PARTNUMBER0: These bits read back as 0x22 | RO | 0x22 |

Table 1109.

SPI: SSPPERIPHID0 Register

Offset: 0xfe0

12.3. SPI
1064

RP2350 Datasheet

Description

Peripheral identification registers, SSPPeriphID0-3 on page 3-13

Table 1110.

SSPPERIPHID0

Register

SPI: SSPPERIPHID1 Register

Offset: 0xfe4

Description

Peripheral identification registers, SSPPeriphID0-3 on page 3-13

| Bits | Description | Type | Reset |
| --- | --- | --- | --- |
| 31:8 | Reserved. | - | - |
| 7:4 | DESIGNER0: These bits read back as 0x1 | RO | 0x1 |
| 3:0 | PARTNUMBER1: These bits read back as 0x0 | RO | 0x0 |

Table 1111.

SSPPERIPHID1

Register

SPI: SSPPERIPHID2 Register

Offset: 0xfe8

Description

Peripheral identification registers, SSPPeriphID0-3 on page 3-13

| Bits | Description | Type | Reset |
| --- | --- | --- | --- |
| 31:8 | Reserved. | - | - |
| 7:4 | REVISION: These bits return the peripheral revision | RO | 0x3 |
| 3:0 | DESIGNER1: These bits read back as 0x4 | RO | 0x4 |

Table 1112.

SSPPERIPHID2

Register

SPI: SSPPERIPHID3 Register

Offset: 0xfec

Description

Peripheral identification registers, SSPPeriphID0-3 on page 3-13

| Bits | Description | Type | Reset |
| --- | --- | --- | --- |
| 31:8 | Reserved. | - | - |
| 7:0 | CONFIGURATION: These bits read back as 0x00 | RO | 0x00 |
| 31:8 | Reserved. | - | - |
| 7:0 | SSPPCELLID0: These bits read back as 0x0D | RO | 0x0d |

Table 1113.

SSPPERIPHID3

Register

SPI: SSPPCELLID0 Register

Offset: 0xff0

Description

PrimeCell identification registers, SSPPCellID0-3 on page 3-16

12.3. SPI
1065

RP2350 Datasheet

Table 1114.

SPI: SSPPCELLID1 Register

Offset: 0xff4

Description

PrimeCell identification registers, SSPPCellID0-3 on page 3-16

| Bits | Description | Type | Reset |
| --- | --- | --- | --- |
| 31:8 | Reserved. | - | - |
| 7:0 | SSPPCELLID1: These bits read back as 0xF0 | RO | 0xf0 |

Table 1115.

SPI: SSPPCELLID2 Register

Offset: 0xff8

Description

PrimeCell identification registers, SSPPCellID0-3 on page 3-16

| Bits | Description | Type | Reset |
| --- | --- | --- | --- |
| 31:8 | Reserved. | - | - |
| 7:0 | SSPPCELLID2: These bits read back as 0x05 | RO | 0x05 |

Table 1116.

SPI: SSPPCELLID3 Register

Offset: 0xffc

Description

PrimeCell identification registers, SSPPCellID0-3 on page 3-16

| Bits | Description | Type | Reset |
| --- | --- | --- | --- |
| 31:8 | Reserved. | - | - |
| 7:0 | SSPPCELLID3: These bits read back as 0xB1 | RO | 0xb1 |

Table 1117.

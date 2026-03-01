---
source_pdf: rp2350-datasheet-7.pdf
repository: llm_database
chapter: Chapter 12. Peripherals
section: 12.1.8. List of registers
pages: 973-984
type: technical_spec
generated_at: 2026-03-01T06:07:53.318745+00:00
---

# 12.1.8. List of registers

12.1.8. List of registers

The UART0 and UART1 registers start at base addresses of 0x40070000 and 0x40078000 respectively (defined as

UART0_BASE and UART1_BASE in SDK).

| Offset | Name | Info |
| --- | --- | --- |
| 0x000 | UARTDR | Data Register, UARTDR |
| 0x004 | UARTRSR | Receive Status Register/Error Clear Register, UARTRSR/UARTECR |
| 0x018 | UARTFR | Flag Register, UARTFR |
| 0x020 | UARTILPR | IrDA Low-Power Counter Register, UARTILPR |
| 0x024 | UARTIBRD | Integer Baud Rate Register, UARTIBRD |
| 0x028 | UARTFBRD | Fractional Baud Rate Register, UARTFBRD |
| 0x02c | UARTLCR_H | Line Control Register, UARTLCR_H |
| 0x030 | UARTCR | Control Register, UARTCR |
| 0x034 | UARTIFLS | Interrupt FIFO Level Select Register, UARTIFLS |
| 0x038 | UARTIMSC | Interrupt Mask Set/Clear Register, UARTIMSC |
| 0x03c | UARTRIS | Raw Interrupt Status Register, UARTRIS |
| 0x040 | UARTMIS | Masked Interrupt Status Register, UARTMIS |
| 0x044 | UARTICR | Interrupt Clear Register, UARTICR |
| 0x048 | UARTDMACR | DMA Control Register, UARTDMACR |
| 0xfe0 | UARTPERIPHID0 | UARTPeriphID0 Register |
| 0xfe4 | UARTPERIPHID1 | UARTPeriphID1 Register |
| 0xfe8 | UARTPERIPHID2 | UARTPeriphID2 Register |
| 0xfec | UARTPERIPHID3 | UARTPeriphID3 Register |
| 0xff0 | UARTPCELLID0 | UARTPCellID0 Register |
| 0xff4 | UARTPCELLID1 | UARTPCellID1 Register |
| 0xff8 | UARTPCELLID2 | UARTPCellID2 Register |
| 0xffc | UARTPCELLID3 | UARTPCellID3 Register |

Table 1028. List of

12.1. UART
972

RP2350 Datasheet

UART: UARTDR Register

Offset: 0x000

Description

Data Register, UARTDR

| Bits | Description | Type | Reset |
| --- | --- | --- | --- |
| 31:12 | Reserved. | - | - |
| 11 | OE: Overrun error. This bit is set to 1 if data is received and the receive FIFO is already full. This is cleared to 0 once there is an empty space in the FIFO and a new character can be written to it. | RO | - |
| 10 | BE: Break error. This bit is set to 1 if a break condition was detected, indicating that the received data input was held LOW for longer than a full-word transmission time (defined as start, data, parity and stop bits). In FIFO mode, this error is associated with the character at the top of the FIFO. When a break occurs, only one 0 character is loaded into the FIFO. The next character is only enabled after the receive data input goes to a 1 (marking state), and the next valid start bit is received. | RO | - |
| 9 | PE: Parity error. When set to 1, it indicates that the parity of the received data character does not match the parity that the EPS and SPS bits in the Line Control Register, UARTLCR_H. In FIFO mode, this error is associated with the character at the top of the FIFO. | RO | - |
| 8 | FE: Framing error. When set to 1, it indicates that the received character did not have a valid stop bit (a valid stop bit is 1). In FIFO mode, this error is associated with the character at the top of the FIFO. | RO | - |
| 7:0 | DATA: Receive (read) data character. Transmit (write) data character. | RWF | - |

Table 1029. UARTDR

12.1. UART
973

RP2350 Datasheet

UART: UARTRSR Register

Offset: 0x004

Description

Receive Status Register/Error Clear Register, UARTRSR/UARTECR

| Bits | Description | Type | Reset |
| --- | --- | --- | --- |
| 31:4 | Reserved. | - | - |
| 3 | OE: Overrun error. This bit is set to 1 if data is received and the FIFO is already full. This bit is cleared to 0 by a write to UARTECR. The FIFO contents remain valid because no more data is written when the FIFO is full, only the contents of the shift register are overwritten. The CPU must now read the data, to empty the FIFO. | WC | 0x0 |
| 2 | BE: Break error. This bit is set to 1 if a break condition was detected, indicating that the received data input was held LOW for longer than a full-word transmission time (defined as start, data, parity, and stop bits). This bit is cleared to 0 after a write to UARTECR. In FIFO mode, this error is associated with the character at the top of the FIFO. When a break occurs, only one 0 character is loaded into the FIFO. The next character is only enabled after the receive data input goes to a 1 (marking state) and the next valid start bit is received. | WC | 0x0 |
| 1 | PE: Parity error. When set to 1, it indicates that the parity of the received data character does not match the parity that the EPS and SPS bits in the Line Control Register, UARTLCR_H. This bit is cleared to 0 by a write to UARTECR. In FIFO mode, this error is associated with the character at the top of the FIFO. | WC | 0x0 |
| 0 | FE: Framing error. When set to 1, it indicates that the received character did not have a valid stop bit (a valid stop bit is 1). This bit is cleared to 0 by a write to UARTECR. In FIFO mode, this error is associated with the character at the top of the FIFO. | WC | 0x0 |

Table 1030. UARTRSR

UART: UARTFR Register

Offset: 0x018

Description

Flag Register, UARTFR

| Bits | Description | Type | Reset |
| --- | --- | --- | --- |
| 31:9 | Reserved. | - | - |
| 8 | RI: Ring indicator. This bit is the complement of the UART ring indicator, nUARTRI, modem status input. That is, the bit is 1 when nUARTRI is LOW. | RO | - |
| 7 | TXFE: Transmit FIFO empty. The meaning of this bit depends on the state of the FEN bit in the Line Control Register, UARTLCR_H. If the FIFO is disabled, this bit is set when the transmit holding register is empty. If the FIFO is enabled, the TXFE bit is set when the transmit FIFO is empty. This bit does not indicate if there is data in the transmit shift register. | RO | 0x1 |
| 6 | RXFF: Receive FIFO full. The meaning of this bit depends on the state of the FEN bit in the UARTLCR_H Register. If the FIFO is disabled, this bit is set when the receive holding register is full. If the FIFO is enabled, the RXFF bit is set when the receive FIFO is full. | RO | 0x0 |
| 5 | TXFF: Transmit FIFO full. The meaning of this bit depends on the state of the FEN bit in the UARTLCR_H Register. If the FIFO is disabled, this bit is set when the transmit holding register is full. If the FIFO is enabled, the TXFF bit is set when the transmit FIFO is full. | RO | 0x0 |
| 4 | RXFE: Receive FIFO empty. The meaning of this bit depends on the state of the FEN bit in the UARTLCR_H Register. If the FIFO is disabled, this bit is set when the receive holding register is empty. If the FIFO is enabled, the RXFE bit is set when the receive FIFO is empty. | RO | 0x1 |
| 3 | BUSY: UART busy. If this bit is set to 1, the UART is busy transmitting data. This bit remains set until the complete byte, including all the stop bits, has been sent from the shift register. This bit is set as soon as the transmit FIFO becomes non-empty, regardless of whether the UART is enabled or not. | RO | 0x0 |
| 2 | DCD: Data carrier detect. This bit is the complement of the UART data carrier detect, nUARTDCD, modem status input. That is, the bit is 1 when nUARTDCD is LOW. | RO | - |
| 1 | DSR: Data set ready. This bit is the complement of the UART data set ready, nUARTDSR, modem status input. That is, the bit is 1 when nUARTDSR is LOW. | RO | - |
| 0 | CTS: Clear to send. This bit is the complement of the UART clear to send, nUARTCTS, modem status input. That is, the bit is 1 when nUARTCTS is LOW. | RO | - |

Table 1031. UARTFR

12.1. UART
974

RP2350 Datasheet

UART: UARTILPR Register

Offset: 0x020

Description

IrDA Low-Power Counter Register, UARTILPR

| Bits | Description | Type | Reset |
| --- | --- | --- | --- |
| 31:8 | Reserved. | - | - |
| 7:0 | ILPDVSR: 8-bit low-power divisor value. These bits are cleared to 0 at reset. | RW | 0x00 |

Table 1032. UARTILPR

UART: UARTIBRD Register

Offset: 0x024

Description

Integer Baud Rate Register, UARTIBRD

| Bits | Description | Type | Reset |
| --- | --- | --- | --- |
| 31:16 | Reserved. | - | - |
| 15:0 | BAUD_DIVINT: The integer baud rate divisor. These bits are cleared to 0 on reset. | RW | 0x0000 |

Table 1033. UARTIBRD

12.1. UART
975

RP2350 Datasheet

UART: UARTFBRD Register

Offset: 0x028

Description

Fractional Baud Rate Register, UARTFBRD

| Bits | Description | Type | Reset |
| --- | --- | --- | --- |
| 31:6 | Reserved. | - | - |
| 5:0 | BAUD_DIVFRAC: The fractional baud rate divisor. These bits are cleared to 0 on reset. | RW | 0x00 |

Table 1034.

UART: UARTLCR_H Register

Offset: 0x02c

Description

Line Control Register, UARTLCR_H

| Bits | Description | Type | Reset |
| --- | --- | --- | --- |
| 31:8 | Reserved. | - | - |
| 7 | SPS: Stick parity select. 0 = stick parity is disabled 1 = either: * if the EPS bit is 0 then the parity bit is transmitted and checked as a 1 * if the EPS bit is 1 then the parity bit is transmitted and checked as a 0. This bit has no effect when the PEN bit disables parity checking and generation. | RW | 0x0 |
| 6:5 | WLEN: Word length. These bits indicate the number of data bits transmitted or received in a frame as follows: b11 = 8 bits b10 = 7 bits b01 = 6 bits b00 = 5 bits. | RW | 0x0 |
| 4 | FEN: Enable FIFOs: 0 = FIFOs are disabled (character mode) that is, the FIFOs become 1-byte-deep holding registers 1 = transmit and receive FIFO buffers are enabled (FIFO mode). | RW | 0x0 |
| 3 | STP2: Two stop bits select. If this bit is set to 1, two stop bits are transmitted at the end of the frame. The receive logic does not check for two stop bits being received. | RW | 0x0 |
| 2 | EPS: Even parity select. Controls the type of parity the UART uses during transmission and reception: 0 = odd parity. The UART generates or checks for an odd number of 1s in the data and parity bits. 1 = even parity. The UART generates or checks for an even number of 1s in the data and parity bits. This bit has no effect when the PEN bit disables parity checking and generation. | RW | 0x0 |
| 1 | PEN: Parity enable: 0 = parity is disabled and no parity bit added to the data frame 1 = parity checking and generation is enabled. | RW | 0x0 |
| 0 | BRK: Send break. If this bit is set to 1, a low-level is continually output on the UARTTXD output, after completing transmission of the current character. For the proper execution of the break command, the software must set this bit for at least two complete frames. For normal use, this bit must be cleared to 0. | RW | 0x0 |
| 31:16 | Reserved. | - | - |
| 15 | CTSEN: CTS hardware flow control enable. If this bit is set to 1, CTS hardware flow control is enabled. Data is only transmitted when the nUARTCTS signal is asserted. | RW | 0x0 |
| 14 | RTSEN: RTS hardware flow control enable. If this bit is set to 1, RTS hardware flow control is enabled. Data is only requested when there is space in the receive FIFO for it to be received. | RW | 0x0 |
| 13 | OUT2: This bit is the complement of the UART Out2 (nUARTOut2) modem status output. That is, when the bit is programmed to a 1, the output is 0. For DTE this can be used as Ring Indicator (RI). | RW | 0x0 |
| 12 | OUT1: This bit is the complement of the UART Out1 (nUARTOut1) modem status output. That is, when the bit is programmed to a 1 the output is 0. For DTE this can be used as Data Carrier Detect (DCD). | RW | 0x0 |
| 11 | RTS: Request to send. This bit is the complement of the UART request to send, nUARTRTS, modem status output. That is, when the bit is programmed to a 1 then nUARTRTS is LOW. | RW | 0x0 |
| 10 | DTR: Data transmit ready. This bit is the complement of the UART data transmit ready, nUARTDTR, modem status output. That is, when the bit is programmed to a 1 then nUARTDTR is LOW. | RW | 0x0 |
| 9 | RXE: Receive enable. If this bit is set to 1, the receive section of the UART is enabled. Data reception occurs for either UART signals or SIR signals depending on the setting of the SIREN bit. When the UART is disabled in the middle of reception, it completes the current character before stopping. | RW | 0x1 |
| 8 | TXE: Transmit enable. If this bit is set to 1, the transmit section of the UART is enabled. Data transmission occurs for either UART signals, or SIR signals depending on the setting of the SIREN bit. When the UART is disabled in the middle of transmission, it completes the current character before stopping. | RW | 0x1 |
| 7 | LBE: Loopback enable. If this bit is set to 1 and the SIREN bit is set to 1 and the SIRTEST bit in the Test Control Register, UARTTCR is set to 1, then the nSIROUT path is inverted, and fed through to the SIRIN path. The SIRTEST bit in the test register must be set to 1 to override the normal half-duplex SIR operation. This must be the requirement for accessing the test registers during normal operation, and SIRTEST must be cleared to 0 when loopback testing is finished. This feature reduces the amount of external coupling required during system test. If this bit is set to 1, and the SIRTEST bit is set to 0, the UARTTXD path is fed through to the UARTRXD path. In either SIR mode or UART mode, when this bit is set, the modem outputs are also fed through to the modem inputs. This bit is cleared to 0 on reset, to disable loopback. | RW | 0x0 |
| 6:3 | Reserved. | - | - |
| 2 | SIRLP: SIR low-power IrDA mode. This bit selects the IrDA encoding mode. If this bit is cleared to 0, low-level bits are transmitted as an active high pulse with a width of 3 / 16th of the bit period. If this bit is set to 1, low-level bits are transmitted with a pulse width that is 3 times the period of the IrLPBaud16 input signal, regardless of the selected bit rate. Setting this bit uses less power, but might reduce transmission distances. | RW | 0x0 |
| 1 | SIREN: SIR enable: 0 = IrDA SIR ENDEC is disabled. nSIROUT remains LOW (no light pulse generated), and signal transitions on SIRIN have no effect. 1 = IrDA SIR ENDEC is enabled. Data is transmitted and received on nSIROUT and SIRIN. UARTTXD remains HIGH, in the marking state. Signal transitions on UARTRXD or modem status inputs have no effect. This bit has no effect if the UARTEN bit disables the UART. | RW | 0x0 |
| 0 | UARTEN: UART enable: 0 = UART is disabled. If the UART is disabled in the middle of transmission or reception, it completes the current character before stopping. 1 = the UART is enabled. Data transmission and reception occurs for either UART signals or SIR signals depending on the setting of the SIREN bit. | RW | 0x0 |

Table 1035.

UART: UARTCR Register

12.1. UART
976

RP2350 Datasheet

Offset: 0x030

Description

Control Register, UARTCR

Table 1036. UARTCR

12.1. UART
977

RP2350 Datasheet

UART: UARTIFLS Register

Offset: 0x034

Description

Interrupt FIFO Level Select Register, UARTIFLS

| Bits | Description | Type | Reset |
| --- | --- | --- | --- |
| 31:6 | Reserved. | - | - |
| 5:3 | RXIFLSEL: Receive interrupt FIFO level select. The trigger points for the receive interrupt are as follows: b000 = Receive FIFO becomes >= 1 / 8 full b001 = Receive FIFO becomes >= 1 / 4 full b010 = Receive FIFO becomes >= 1 / 2 full b011 = Receive FIFO becomes >= 3 / 4 full b100 = Receive FIFO becomes >= 7 / 8 full b101-b111 = reserved. | RW | 0x2 |
| 2:0 | TXIFLSEL: Transmit interrupt FIFO level select. The trigger points for the transmit interrupt are as follows: b000 = Transmit FIFO becomes <= 1 / 8 full b001 = Transmit FIFO becomes <= 1 / 4 full b010 = Transmit FIFO becomes <= 1 / 2 full b011 = Transmit FIFO becomes <= 3 / 4 full b100 = Transmit FIFO becomes <= 7 / 8 full b101-b111 = reserved. | RW | 0x2 |

Table 1037. UARTIFLS

UART: UARTIMSC Register

Offset: 0x038

Description

Interrupt Mask Set/Clear Register, UARTIMSC

| Bits | Description | Type | Reset |
| --- | --- | --- | --- |
| 31:11 | Reserved. | - | - |
| 10 | OEIM: Overrun error interrupt mask. A read returns the current mask for the UARTOEINTR interrupt. On a write of 1, the mask of the UARTOEINTR interrupt is set. A write of 0 clears the mask. | RW | 0x0 |
| 9 | BEIM: Break error interrupt mask. A read returns the current mask for the UARTBEINTR interrupt. On a write of 1, the mask of the UARTBEINTR interrupt is set. A write of 0 clears the mask. | RW | 0x0 |
| 8 | PEIM: Parity error interrupt mask. A read returns the current mask for the UARTPEINTR interrupt. On a write of 1, the mask of the UARTPEINTR interrupt is set. A write of 0 clears the mask. | RW | 0x0 |
| 7 | FEIM: Framing error interrupt mask. A read returns the current mask for the UARTFEINTR interrupt. On a write of 1, the mask of the UARTFEINTR interrupt is set. A write of 0 clears the mask. | RW | 0x0 |
| 6 | RTIM: Receive timeout interrupt mask. A read returns the current mask for the UARTRTINTR interrupt. On a write of 1, the mask of the UARTRTINTR interrupt is set. A write of 0 clears the mask. | RW | 0x0 |
| 5 | TXIM: Transmit interrupt mask. A read returns the current mask for the UARTTXINTR interrupt. On a write of 1, the mask of the UARTTXINTR interrupt is set. A write of 0 clears the mask. | RW | 0x0 |
| 4 | RXIM: Receive interrupt mask. A read returns the current mask for the UARTRXINTR interrupt. On a write of 1, the mask of the UARTRXINTR interrupt is set. A write of 0 clears the mask. | RW | 0x0 |
| 3 | DSRMIM: nUARTDSR modem interrupt mask. A read returns the current mask for the UARTDSRINTR interrupt. On a write of 1, the mask of the UARTDSRINTR interrupt is set. A write of 0 clears the mask. | RW | 0x0 |
| 2 | DCDMIM: nUARTDCD modem interrupt mask. A read returns the current mask for the UARTDCDINTR interrupt. On a write of 1, the mask of the UARTDCDINTR interrupt is set. A write of 0 clears the mask. | RW | 0x0 |
| 1 | CTSMIM: nUARTCTS modem interrupt mask. A read returns the current mask for the UARTCTSINTR interrupt. On a write of 1, the mask of the UARTCTSINTR interrupt is set. A write of 0 clears the mask. | RW | 0x0 |
| 0 | RIMIM: nUARTRI modem interrupt mask. A read returns the current mask for the UARTRIINTR interrupt. On a write of 1, the mask of the UARTRIINTR interrupt is set. A write of 0 clears the mask. | RW | 0x0 |

Table 1038.

12.1. UART
978

RP2350 Datasheet

UART: UARTRIS Register

Offset: 0x03c

Description

Raw Interrupt Status Register, UARTRIS

| Bits | Description | Type | Reset |
| --- | --- | --- | --- |
| 31:11 | Reserved. | - | - |
| 10 | OERIS: Overrun error interrupt status. Returns the raw interrupt state of the UARTOEINTR interrupt. | RO | 0x0 |
| 9 | BERIS: Break error interrupt status. Returns the raw interrupt state of the UARTBEINTR interrupt. | RO | 0x0 |
| 8 | PERIS: Parity error interrupt status. Returns the raw interrupt state of the UARTPEINTR interrupt. | RO | 0x0 |
| 7 | FERIS: Framing error interrupt status. Returns the raw interrupt state of the UARTFEINTR interrupt. | RO | 0x0 |
| 6 | RTRIS: Receive timeout interrupt status. Returns the raw interrupt state of the UARTRTINTR interrupt. a | RO | 0x0 |
| 5 | TXRIS: Transmit interrupt status. Returns the raw interrupt state of the UARTTXINTR interrupt. | RO | 0x0 |
| 4 | RXRIS: Receive interrupt status. Returns the raw interrupt state of the UARTRXINTR interrupt. | RO | 0x0 |
| 3 | DSRRMIS: nUARTDSR modem interrupt status. Returns the raw interrupt state of the UARTDSRINTR interrupt. | RO | - |
| 2 | DCDRMIS: nUARTDCD modem interrupt status. Returns the raw interrupt state of the UARTDCDINTR interrupt. | RO | - |
| 1 | CTSRMIS: nUARTCTS modem interrupt status. Returns the raw interrupt state of the UARTCTSINTR interrupt. | RO | - |
| 0 | RIRMIS: nUARTRI modem interrupt status. Returns the raw interrupt state of the UARTRIINTR interrupt. | RO | - |

Table 1039. UARTRIS

12.1. UART
979

RP2350 Datasheet

UART: UARTMIS Register

Offset: 0x040

Description

Masked Interrupt Status Register, UARTMIS

| Bits | Description | Type | Reset |
| --- | --- | --- | --- |
| 31:11 | Reserved. | - | - |
| 10 | OEMIS: Overrun error masked interrupt status. Returns the masked interrupt state of the UARTOEINTR interrupt. | RO | 0x0 |
| 9 | BEMIS: Break error masked interrupt status. Returns the masked interrupt state of the UARTBEINTR interrupt. | RO | 0x0 |
| 8 | PEMIS: Parity error masked interrupt status. Returns the masked interrupt state of the UARTPEINTR interrupt. | RO | 0x0 |
| 7 | FEMIS: Framing error masked interrupt status. Returns the masked interrupt state of the UARTFEINTR interrupt. | RO | 0x0 |
| 6 | RTMIS: Receive timeout masked interrupt status. Returns the masked interrupt state of the UARTRTINTR interrupt. | RO | 0x0 |
| 5 | TXMIS: Transmit masked interrupt status. Returns the masked interrupt state of the UARTTXINTR interrupt. | RO | 0x0 |
| 4 | RXMIS: Receive masked interrupt status. Returns the masked interrupt state of the UARTRXINTR interrupt. | RO | 0x0 |
| 3 | DSRMMIS: nUARTDSR modem masked interrupt status. Returns the masked interrupt state of the UARTDSRINTR interrupt. | RO | - |
| 2 | DCDMMIS: nUARTDCD modem masked interrupt status. Returns the masked interrupt state of the UARTDCDINTR interrupt. | RO | - |
| 1 | CTSMMIS: nUARTCTS modem masked interrupt status. Returns the masked interrupt state of the UARTCTSINTR interrupt. | RO | - |
| 0 | RIMMIS: nUARTRI modem masked interrupt status. Returns the masked interrupt state of the UARTRIINTR interrupt. | RO | - |

Table 1040. UARTMIS

12.1. UART
980

RP2350 Datasheet

UART: UARTICR Register

Offset: 0x044

Description

Interrupt Clear Register, UARTICR

| Bits | Description | Type | Reset |
| --- | --- | --- | --- |
| 31:11 | Reserved. | - | - |
| 10 | OEIC: Overrun error interrupt clear. Clears the UARTOEINTR interrupt. | WC | - |
| 9 | BEIC: Break error interrupt clear. Clears the UARTBEINTR interrupt. | WC | - |
| 8 | PEIC: Parity error interrupt clear. Clears the UARTPEINTR interrupt. | WC | - |
| 7 | FEIC: Framing error interrupt clear. Clears the UARTFEINTR interrupt. | WC | - |
| 6 | RTIC: Receive timeout interrupt clear. Clears the UARTRTINTR interrupt. | WC | - |
| 5 | TXIC: Transmit interrupt clear. Clears the UARTTXINTR interrupt. | WC | - |
| 4 | RXIC: Receive interrupt clear. Clears the UARTRXINTR interrupt. | WC | - |
| 3 | DSRMIC: nUARTDSR modem interrupt clear. Clears the UARTDSRINTR interrupt. | WC | - |
| 2 | DCDMIC: nUARTDCD modem interrupt clear. Clears the UARTDCDINTR interrupt. | WC | - |
| 1 | CTSMIC: nUARTCTS modem interrupt clear. Clears the UARTCTSINTR interrupt. | WC | - |
| 0 | RIMIC: nUARTRI modem interrupt clear. Clears the UARTRIINTR interrupt. | WC | - |

Table 1041. UARTICR

UART: UARTDMACR Register

Offset: 0x048

Description

DMA Control Register, UARTDMACR

| Bits | Description | Type | Reset |
| --- | --- | --- | --- |
| 31:3 | Reserved. | - | - |
| 2 | DMAONERR: DMA on error. If this bit is set to 1, the DMA receive request outputs, UARTRXDMASREQ or UARTRXDMABREQ, are disabled when the UART error interrupt is asserted. | RW | 0x0 |
| 1 | TXDMAE: Transmit DMA enable. If this bit is set to 1, DMA for the transmit FIFO is enabled. | RW | 0x0 |
| 0 | RXDMAE: Receive DMA enable. If this bit is set to 1, DMA for the receive FIFO is enabled. | RW | 0x0 |
| 31:8 | Reserved. | - | - |
| 7:0 | PARTNUMBER0: These bits read back as 0x11 | RO | 0x11 |

Table 1042.

12.1. UART
981

RP2350 Datasheet

UART: UARTPERIPHID0 Register

Offset: 0xfe0

Description

UARTPeriphID0 Register

Table 1043.

UARTPERIPHID0

Register

UART: UARTPERIPHID1 Register

Offset: 0xfe4

Description

UARTPeriphID1 Register

| Bits | Description | Type | Reset |
| --- | --- | --- | --- |
| 31:8 | Reserved. | - | - |
| 7:4 | DESIGNER0: These bits read back as 0x1 | RO | 0x1 |
| 3:0 | PARTNUMBER1: These bits read back as 0x0 | RO | 0x0 |

Table 1044.

UARTPERIPHID1

Register

UART: UARTPERIPHID2 Register

Offset: 0xfe8

Description

UARTPeriphID2 Register

| Bits | Description | Type | Reset |
| --- | --- | --- | --- |
| 31:8 | Reserved. | - | - |
| 7:4 | REVISION: This field depends on the revision of the UART: r1p0 0x0 r1p1 0x1 r1p3 0x2 r1p4 0x2 r1p5 0x3 | RO | 0x3 |
| 3:0 | DESIGNER1: These bits read back as 0x4 | RO | 0x4 |

Table 1045.

UARTPERIPHID2

Register

UART: UARTPERIPHID3 Register

Offset: 0xfec

Description

UARTPeriphID3 Register

| Bits | Description | Type | Reset |
| --- | --- | --- | --- |
| 31:8 | Reserved. | - | - |
| 7:0 | CONFIGURATION: These bits read back as 0x00 | RO | 0x00 |
| 31:8 | Reserved. | - | - |
| 7:0 | UARTPCELLID0: These bits read back as 0x0D | RO | 0x0d |

Table 1046.

UARTPERIPHID3

Register

UART: UARTPCELLID0 Register

Offset: 0xff0

Description

UARTPCellID0 Register

12.1. UART
982

RP2350 Datasheet

Table 1047.

UARTPCELLID0

Register

UART: UARTPCELLID1 Register

Offset: 0xff4

Description

UARTPCellID1 Register

| Bits | Description | Type | Reset |
| --- | --- | --- | --- |
| 31:8 | Reserved. | - | - |
| 7:0 | UARTPCELLID1: These bits read back as 0xF0 | RO | 0xf0 |

Table 1048.

UARTPCELLID1

Register

UART: UARTPCELLID2 Register

Offset: 0xff8

Description

UARTPCellID2 Register

| Bits | Description | Type | Reset |
| --- | --- | --- | --- |
| 31:8 | Reserved. | - | - |
| 7:0 | UARTPCELLID2: These bits read back as 0x05 | RO | 0x05 |

Table 1049.

UARTPCELLID2

Register

UART: UARTPCELLID3 Register

Offset: 0xffc

Description

UARTPCellID3 Register

| Bits | Description | Type | Reset |
| --- | --- | --- | --- |
| 31:8 | Reserved. | - | - |
| 7:0 | UARTPCELLID3: These bits read back as 0xB1 | RO | 0xb1 |

Table 1050.

UARTPCELLID3

Register

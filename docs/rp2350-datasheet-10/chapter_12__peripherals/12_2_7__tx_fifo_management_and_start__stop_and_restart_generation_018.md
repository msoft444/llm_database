---
source_pdf: rp2350-datasheet-10.pdf
repository: llm_database
chapter: Chapter 12. Peripherals
section: 12.2.7. TX FIFO Management and START, STOP and RESTART Generation
pages: 994-996
type: technical_spec
generated_at: 2026-03-01T12:20:14.494106+00:00
---

# 12.2.7. TX FIFO Management and START, STOP and RESTART Generation

RP2350 Datasheet

12.2.7. TX FIFO Management and START, STOP and RESTART Generation

When operating as a master, the DW_apb_i2c component supports the mode of TX (transmit) FIFO management

illustrated in Figure 76.

12.2.7.1. TX FIFO management

The component does not generate a STOP if the TX FIFO becomes empty; in this situation the component holds the SCL

line low, stalling the bus until a new entry is available in the TX FIFO. A STOP condition is generated only when the user

specifically requests it by setting bit nine (Stop bit) of the command written to IC_DATA_CMD register. Figure 76 shows

the bits in the IC_DATA_CMD register.

![Page 994 figure](images/fig_p0994.png)

*Figure 76. IC_DATA_CMD Register*

*Figure 77. Master Transmitter - TX FIFO Empties/STOP Generation Receiver - TX FIFO Empties/STOP Generation as a master transmitter.*

Data 
Read/Write field; data retrieved from slave is read from  
this field; data to be sent to slave is written to this field

Figure 77 illustrates the behaviour of the DW_apb_i2c when the TX FIFO becomes empty while operating as a master

transmitter, as well as the generation of a STOP condition.

Figure 78 illustrates the behaviour of the DW_apb_i2c when the TX FIFO becomes empty while operating as a master

receiver, as well as the generation of a STOP condition.

S
Figure 78. Master

Figure 79 and Figure 80 illustrate configurations where the user can control the generation of RESTART conditions on

the I2C bus. If bit 10 (Restart) of the IC_DATA_CMD register is set and the restart capability is enabled (IC_RESTART_EN=1),

a RESTART is generated before the data byte is written to or read from the slave. If the restart capability is not enabled,

a STOP followed by a START is generated in place of the RESTART. Figure 79 illustrates this situation during operation

12.2. I2C
993

RP2350 Datasheet

![Page 995 figure](images/fig_p0995.png)

*Figure 79. Master Transmitter - Restart Bit of IC_DATA_CMD Is Set Receiver - Restart Bit of IC_DATA_CMD Is Set FIFO is not empty.*

*Figure 81. Master Transmitter - Stop Bit of IC_DATA_CMD Set/TX FIFO not empty*

*Figure 82. Master Transmitter - First Byte Loaded Into TX FIFO Allowed to Empty, Restart Bit Set FIFO is not empty. Receiver - Stop Bit of IC_DATA_CMD Set/TX FIFO Not Empty*

D7
D6
D5
D4
D3
D2
D1
D0
D7
D6
D5
D4
D3
D2
D1
D0
A5
A4
A3
A2
A1
A0
A6
A5
A4
A3
A2
A1
A0
D6
D7
W Ack
Ack
Ack
W
Ack

Figure 80 illustrates the same situation, but during operation as a master receiver.

SR
Figure 80. Master

D7
D6
D5
D4
D3
D2
D1
D0
D7
D6
D5
D4
D3
D2
D1
D0
A5
A4
A3
A2
A1
A0
A6
A5
A4
A3
A2
A1
A0
D7
D6
R
R
Ack
Ack
Nak
Ack

Figure 81 illustrates operation as a master transmitter where the Stop bit of the IC_DATA_CMD register is set and the TX

D7
D6
D5
D4
D3
D2
D1
D0
D7
D6
D5
D4
D3
D2
D1
D0
A5
A4
A3
A2
A1
A0
A6
A5
A4
A3
A2
A1
A0
D6
D7
W Ack
Ack
Ack
W
Ack

Because more data is available in 
Tx FIFO, a new transmission is 
immediately initiated  (provided 
master is granted access to bus)

Figure 82 illustrates operation as a master transmitter where the first byte loaded into the TX FIFO is allowed to go

empty with the Restart bit set.

D7
D6
D5
D4
D3
D2
D1
D0
D7
D6
D5
D4
D3
D2
D1
D0
A5
A4
A3
A2
A1
A0
A6
A5
A4
A3
A2
A1
A0
D6
D7
W Ack
Ack
Ack
W
Ack

Figure 83 illustrates operation as a master receiver where the Stop bit of the IC_DATA_CMD register is set and the TX

Nak
Figure 83. Master

D7
D6
D5
D4
D3
D2
D1
D0
D7
D6
D5
D4
D3
D2
D1
D0
A5
A4
A3
A2
A1
A0
A6
A5
A4
A3
A2
A1
A0
D7
D6
R
R
Ack
Ack
Ack

Figure 84 illustrates operation as a master receiver where the first command loaded after the TX FIFO is allowed to

empty and the Restart bit is set.

12.2. I2C
994

RP2350 Datasheet

![Page 996 figure](images/fig_p0996.png)

SR
Figure 84. Master

D7
D6
D5
D4
D3
D2
D1
D0
D7
D6
D5
D4
D3
D2
D1
D0
A5
A4
A3
A2
A1
A0
A6
A5
A4
A3
A2
A1
A0
D7
D6
R
R
Ack
Ack
Nak
Ack

---
source_pdf: rp2350-datasheet-1.pdf
repository: llm_database
chapter: Chapter 12. Peripherals
section: 12.2.7. TX FIFO Management and START, STOP and RESTART Generation
pages: 994-995
type: technical_spec
generated_at: 2026-02-28T17:22:56.644477+00:00
---

# 12.2.7. TX FIFO Management and START, STOP and RESTART Generation

12.2.7. TX FIFO Management and START, STOP and RESTART Generation
When operating as a master, the DW_apb_i2c component supports the mode of TX (transmit) FIFO management
illustrated in Figure 76.
12.2.7.1. TX FIFO management
The component does not generate a STOP if the TX FIFO becomes empty; in this situation the component holds the SCL
line low, stalling the bus until a new entry is available in the TX FIFO. A STOP condition is generated only when the user
specifically requests it by setting bit nine (Stop bit) of the command written to IC_DATA_CMD register. Figure 76 shows
the bits in the IC_DATA_CMD register.
IC_DATA_CMD Restart
Data 
Read/Write field; data retrieved from slave is read from  
 
this field; data to be sent to slave is written to this field
CDM 
Write-only field; this bit determines whether transfer to  
 
be carried out is Read (CMD=1) or Write (CMD=0)
Stop 
Write-only field; this bit determines whether STOP is  
 
generated after data byte is sent or received
Restart 
Write-only field; this bit determines whether RESTART  
 
(or STOP followed by START in case or restart   
 
capability is not enabled) is generated before data is  
 
sent or received
9
8
7
0
Stop
CMD
DATA
Figure 76.
IC_DATA_CMD
Register
Figure 77 illustrates the behaviour of the DW_apb_i2c when the TX FIFO becomes empty while operating as a master
transmitter, as well as the generation of a STOP condition.
SDA
SCL
FIFO_
EMPTY
A6
S
Tx FIFO loaded with data 
(write data in this example)
Last byte popped from 
Tx FIFO, with STOP bit 
not set
Master releases SCL line and 
resumes transmission because 
new data became available
Data availability triggers 
START condition on bus
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
D6
D7
D5
D4
D3
D2
D1
D0
W Ack
Ack
Ack
Ack
P
Because STOP bit was not set on 
last byte popped from Tx FIFO, 
Master holds SCL low
Tx FIFO loaded 
with new data
Last byte popped from Tx FIFO 
with STOP bit set
STOP bit enabled triggers 
STOP condition on bus
Figure 77. Master
Transmitter - TX FIFO
Empties/STOP
Generation
Figure 78 illustrates the behaviour of the DW_apb_i2c when the TX FIFO becomes empty while operating as a master
receiver, as well as the generation of a STOP condition.
SDA
SCL
FIFO_
EMPTY
A6
S
Tx FIFO loaded with command 
(read operation in this example)
Last command 
popped from Tx 
FIFO, with STOP bit 
not set
Tx FIFO loaded 
with new command
Last command popped from 
Tx FIFO with STOP bit set
STOP bit enabled triggers 
STOP condition on bus
Master releases SCL line and 
resumes transmission 
because new command 
became available
Because STOP bit was 
not set on last 
command popped 
from Tx FIFO, Master 
holds SCL low
Command availability triggers 
START condition on bus
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
D7
D6
D5
D4
D3
D2
D1
D0
R
Ack
Ack
Nak
Ack
S
Figure 78. Master
Receiver - TX FIFO
Empties/STOP
Generation
Figure 79 and Figure 80 illustrate configurations where the user can control the generation of RESTART conditions on
the I2C bus. If bit 10 (Restart) of the IC_DATA_CMD register is set and the restart capability is enabled (IC_RESTART_EN=1),
a RESTART is generated before the data byte is written to or read from the slave. If the restart capability is not enabled,
a STOP followed by a START is generated in place of the RESTART. Figure 79 illustrates this situation during operation
as a master transmitter.
RP2350 Datasheet
12.2. I2C
993


SDA
SCL
FIFO_
EMPTY
A6
S
Next byte in Tx FIFO 
has RESTART bit set
Because next byte on Tx FIFO has 
been tagged with RESTART bit, 
Master issues RESTART and 
initiates new transmission
Data availability triggers 
START condition on bus
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
SR
Tx FIFO loaded with data 
(write data in this example)
Figure 79. Master
Transmitter - Restart
Bit of IC_DATA_CMD
Is Set
Figure 80 illustrates the same situation, but during operation as a master receiver.
SDA
SCL
FIFO_
EMPTY
A6
S
Tx FIFO loaded with command 
(read operation in this example)
Next command in Tx FIFO 
has RESTART bit set
Master issues NOT ACK as 
required before RESTART 
when operating as receiver
Because next command on Tx FIFO 
has been tagged with RESTART bit, 
Master issues RESTART and 
initiates new transmission 
Command availability triggers 
START condition on bus
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
SR
Figure 80. Master
Receiver - Restart Bit
of IC_DATA_CMD Is
Set
Figure 81 illustrates operation as a master transmitter where the Stop bit of the IC_DATA_CMD register is set and the TX
FIFO is not empty.
SDA
SCL
FIFO_
EMPTY
A6
S
Tx FIFO loaded with data 
(write data in this example)
One byte (not last one) 
is popped from Tx FIFO 
with STOP bit set
Because more data is available in 
Tx FIFO, a new transmission is 
immediately initiated  (provided 
master is granted access to bus)
Data availability triggers 
START condition on bus
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
S
P
Because STOP bit was set on last 
byte popped from Tx FIFO, Master 
generates STOP condition
Figure 81. Master
Transmitter - Stop Bit
of IC_DATA_CMD
Set/TX FIFO not empty
Figure 82 illustrates operation as a master transmitter where the first byte loaded into the TX FIFO is allowed to go
empty with the Restart bit set.
SDA
SCL
FIFO_
EMPTY
A6
S
Last byte popped 
from Tx FIFO with 
STOP bit not set
Tx FIFO loaded 
with new command
Master issues RESTART and 
initiates new transmission
Because STOP bit was 
not set on last byte 
popped from Tx FIFO, 
Master holds SCL low
Data availability triggers START 
condition on bus
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
SR
Tx FIFO loaded with data 
(write data in this example)
Figure 82. Master
Transmitter - First
Byte Loaded Into TX
FIFO Allowed to
Empty, Restart Bit Set
Figure 83 illustrates operation as a master receiver where the Stop bit of the IC_DATA_CMD register is set and the TX
FIFO is not empty.
SDA
SCL
FIFO_
EMPTY
A6
S
Tx FIFO loaded with command 
(read operation in this example)
One command 
(not last one) is 
popped from 
Tx FIFO with 
STOP bit set
Because more commands 
are available inTx FIFO, a 
new transmission is 
immediately initiated 
(provided master is granted 
access to bus)
Because STOP bit was 
set on last command 
popped from Tx FIFO, 
Master generates 
STOP condition
Command availability triggers 
START condition on bus
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
S
P
Nak
Figure 83. Master
Receiver - Stop Bit of
IC_DATA_CMD Set/TX
FIFO Not Empty
Figure 84 illustrates operation as a master receiver where the first command loaded after the TX FIFO is allowed to
empty and the Restart bit is set.
RP2350 Datasheet
12.2. I2C
994


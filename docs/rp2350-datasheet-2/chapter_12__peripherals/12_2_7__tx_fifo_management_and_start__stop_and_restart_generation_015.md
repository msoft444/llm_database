---
source_pdf: rp2350-datasheet-2.pdf
repository: llm_database
chapter: Chapter 12. Peripherals
section: 12.2.7. TX FIFO Management and START, STOP and RESTART Generation
pages: 994-995
type: technical_spec
generated_at: 2026-02-28T17:43:10.557178+00:00
---

# 12.2.7. TX FIFO Management and START, STOP and RESTART Generation

![Page 994 figure](images/fig_p0994.png)

RP2350 Datasheet

12.2.7. TX FIFO Management and START, STOP and RESTART Generation

When operating as a master, the DW_apb_i2c component supports the mode of TX (transmit) FIFO management

illustrated in Figure 76.

12.2.7.1. TX FIFO management

The component does not generate a STOP if the TX FIFO becomes empty; in this situation the component holds the SCL

line low, stalling the bus until a new entry is available in the TX FIFO. A STOP condition is generated only when the user

specifically requests it by setting bit nine (Stop bit) of the command written to IC_DATA_CMD register. Figure 76 shows

the bits in the IC_DATA_CMD register.

Figure 76.

IC_DATA_CMD

Stop | CMD | DATA

IC_DATA_CMD Restart

Register

9
8
7
0

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

Figure 77 illustrates the behaviour of the DW_apb_i2c when the TX FIFO becomes empty while operating as a master

transmitter, as well as the generation of a STOP condition.

Figure 77. Master

S

P

A6 A | 5 A4 | A3 | A2 A | 1 A0

7 D6 | D5 | D4 D | 3 D2 | D1

ckD7 | D6 D | 5 D4 | D3 | D2 D1

D7 D6 D5 | D4 | D3 D | 2 D1

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

Transmitter - TX FIFO

SDA

Empties/STOP

SCL

Generation

FIFO_
EMPTY

STOP bit enabled triggers 
STOP condition on bus

Data availability triggers 
START condition on bus

Last byte popped from 
Tx FIFO, with STOP bit 
not set

Master releases SCL line and 
resumes transmission because 
new data became available

Because STOP bit was not set on 
last byte popped from Tx FIFO, 
Master holds SCL low

Tx FIFO loaded 
with new data

Tx FIFO loaded with data 
(write data in this example)

Last byte popped from Tx FIFO 
with STOP bit set

Figure 78 illustrates the behaviour of the DW_apb_i2c when the TX FIFO becomes empty while operating as a master

receiver, as well as the generation of a STOP condition.

S
Figure 78. Master

S

A6 A | 5 A4 | A3 | A2 A | 1 A0

7 D6 | D5 | D4 D | 3 D2 | D1

ckD7 | D6 D | 5 D4 | D3 | D2 D1

D7 D6 D5 | D4 | D3 D | 2 D1

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

Receiver - TX FIFO

SDA

Empties/STOP

SCL

Generation

FIFO_
EMPTY

Because STOP bit was 
not set on last 
command popped 
from Tx FIFO, Master 
holds SCL low
Command availability triggers 
START condition on bus

STOP bit enabled triggers 
STOP condition on bus

Master releases SCL line and 
resumes transmission 
because new command 
became available

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

Figure 79 and Figure 80 illustrate configurations where the user can control the generation of RESTART conditions on

the I2C bus. If bit 10 (Restart) of the IC_DATA_CMD register is set and the restart capability is enabled (IC_RESTART_EN=1),

a RESTART is generated before the data byte is written to or read from the slave. If the restart capability is not enabled,

a STOP followed by a START is generated in place of the RESTART. Figure 79 illustrates this situation during operation

as a master transmitter.

12.2. I2C
993

![Page 995 figure](images/fig_p0995.png)

RP2350 Datasheet

Figure 79. Master

S

SR

A6 A | 5 A4 | A3 | A2 A | 1 A0

7 D6 | D5 | D4 D | 3 D2 | D1

kD7 | D6 D | 5 D4 D | 3 D2 D1

A6 A5 A4 A3 | A2 A1

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

Transmitter - Restart

SDA

Bit of IC_DATA_CMD

SCL

Is Set

FIFO_
EMPTY

Data availability triggers 
START condition on bus

Because next byte on Tx FIFO has 
been tagged with RESTART bit, 
Master issues RESTART and 
initiates new transmission

Next byte in Tx FIFO 
has RESTART bit set

Tx FIFO loaded with data 
(write data in this example)

Figure 80 illustrates the same situation, but during operation as a master receiver.

SR
Figure 80. Master

S

A6 A | 5 A4 | A3 | A2 A | 1 A0

7 D6 | D5 | D4 D | 3 D2 | D1

kD7 | D6 D | 5 D4 D | 3 D2 D1

A6 A5 A4 A3 | A2 A1

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

Receiver - Restart Bit

SDA

of IC_DATA_CMD Is

SCL

Set

FIFO_
EMPTY

Because next command on Tx FIFO 
has been tagged with RESTART bit, 
Master issues RESTART and 
initiates new transmission 

Command availability triggers 
START condition on bus

Tx FIFO loaded with command 
(read operation in this example)

Next command in Tx FIFO 
has RESTART bit set

Master issues NOT ACK as 
required before RESTART 
when operating as receiver

Figure 81 illustrates operation as a master transmitter where the Stop bit of the IC_DATA_CMD register is set and the TX

FIFO is not empty.

Figure 81. Master

S

S
P

A6 A | 5 A4 | A3 | A2 A | 1 A0

7 D6 | D5 | D4 D | 3 D2 | D1

kD7 | D6 D | 5 D4 D | 3 D2 D1

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

Transmitter - Stop Bit

SDA

of IC_DATA_CMD

SCL

Set/TX FIFO not empty

FIFO_
EMPTY

Data availability triggers 
START condition on bus

One byte (not last one) 
is popped from Tx FIFO 
with STOP bit set

Because more data is available in 
Tx FIFO, a new transmission is 
immediately initiated  (provided 
master is granted access to bus)

Because STOP bit was set on last 
byte popped from Tx FIFO, Master 
generates STOP condition

Tx FIFO loaded with data 
(write data in this example)

Figure 82 illustrates operation as a master transmitter where the first byte loaded into the TX FIFO is allowed to go

empty with the Restart bit set.

Figure 82. Master

S

SR

A6 A | 5 A4 | A3 | A2 A | 1 A0

7 D6 | D5 | D4 D | 3 D2 | D1

kD7 | D6 D | 5 D4 D | 3 D2 D1

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

Transmitter - First

SDA

Byte Loaded Into TX

SCL

FIFO Allowed to

FIFO_
EMPTY

Empty, Restart Bit Set

Because STOP bit was 
not set on last byte 
popped from Tx FIFO, 
Master holds SCL low
Data availability triggers START 
condition on bus

Master issues RESTART and 
initiates new transmission

Last byte popped 
from Tx FIFO with 
STOP bit not set

Tx FIFO loaded 
with new command

Tx FIFO loaded with data 
(write data in this example)

Figure 83 illustrates operation as a master receiver where the Stop bit of the IC_DATA_CMD register is set and the TX

FIFO is not empty.

Nak
Figure 83. Master

S

S
P

A6 A | 5 A4 | A3 | A2 A | 1 A0

7 D6 | D5 | D4 D | 3 D2 | D1

kD7 | D6 D | 5 D4 D | 3 D2 D1

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

Receiver - Stop Bit of

SDA

IC_DATA_CMD Set/TX

SCL

FIFO Not Empty

FIFO_
EMPTY

Because STOP bit was 
set on last command 
popped from Tx FIFO, 
Master generates 
STOP condition

Because more commands 
are available inTx FIFO, a 
new transmission is 
immediately initiated 
(provided master is granted 
access to bus)

Command availability triggers 
START condition on bus

Tx FIFO loaded with command 
(read operation in this example)

One command 
(not last one) is 
popped from 
Tx FIFO with 
STOP bit set

Figure 84 illustrates operation as a master receiver where the first command loaded after the TX FIFO is allowed to

empty and the Restart bit is set.

12.2. I2C
994

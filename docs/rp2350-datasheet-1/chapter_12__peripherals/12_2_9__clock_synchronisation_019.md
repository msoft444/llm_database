---
source_pdf: rp2350-datasheet-1.pdf
repository: llm_database
chapter: Chapter 12. Peripherals
section: 12.2.9. Clock synchronisation
pages: 996-996
type: technical_spec
generated_at: 2026-02-28T17:22:56.644477+00:00
---

# 12.2.9. Clock synchronisation

SDA
SCL
FIFO_
EMPTY
A6
S
Tx FIFO loaded with command 
(read operation in this example)
Last command popped 
from Tx FIFO with 
STOP bit not set
Tx FIFO loaded 
with new command
Next command loaded into 
Tx FIFO has RESTART bit set
Master issues NOT ACK as 
required before RESTART 
when operating as receiver
Master issues RESTART and 
initiates new transmission
Because STOP bit 
was not set on last 
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
Figure 84. Master
Receiver - First
Command Loaded
After TX FIFO Allowed
to Empty/Restart Bit
Set
12.2.8. Multiple master arbitration
The DW_apb_i2c bus protocol allows multiple masters to reside on the same bus. If there are two masters on the same
I2C bus, there is an arbitration procedure if both try to take control of the bus at the same time by generating a START
condition at the same time. Once a master (for example, a microcontroller) has control of the bus, no other master can
take control until the first master sends a STOP condition and places the bus in an idle state.
Arbitration takes place on the SDA line, while the SCL line is set to 1. The master, which transmits a one while the other
master transmits zero, loses arbitration and turns off its data output stage. The master that lost arbitration can continue
to generate clocks until the end of the byte transfer. If both masters address the same slave device, the arbitration
could go into the data phase.
Upon detecting that it has lost arbitration to another master, the DW_apb_i2c stops generating SCL by disabling the output
driver. Figure 85 illustrates the timing of two masters arbitrating on the bus.
CLKA
DATA2
SDA
SCL
MSB
MSB
MSB
‘0’
matching data
DATA1 loses arbitration
SDA mirrors DATA2
SDA lines up 
with DATA1 
START condition
‘1’
Figure 85. Multiple
Master Arbitration
Control of the bus is determined by address or master code and data sent by competing masters, so there is no central
master nor any order of priority on the bus.
Arbitration is not allowed between the following conditions:
• A RESTART condition and a data bit
• A STOP condition and a data bit
• A RESTART condition and a STOP condition
NOTE
Slaves do not participate in the arbitration process.
12.2.9. Clock synchronisation
When two or more masters try to transfer information on the bus at the same time, they must arbitrate and synchronize
the SCL clock. All masters generate their own clock to transfer messages. Data is valid only during the high period of SCL
RP2350 Datasheet
12.2. I2C
995


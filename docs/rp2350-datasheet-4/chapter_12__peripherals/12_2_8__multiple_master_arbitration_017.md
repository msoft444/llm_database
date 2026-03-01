---
source_pdf: rp2350-datasheet-4.pdf
repository: llm_database
chapter: Chapter 12. Peripherals
section: 12.2.8. Multiple master arbitration
pages: 996-996
type: technical_spec
generated_at: 2026-03-01T03:27:49.792955+00:00
---

# 12.2.8. Multiple master arbitration

![Page 996 figure](images/fig_p0996.png)

12.2.8. Multiple master arbitration

The DW_apb_i2c bus protocol allows multiple masters to reside on the same bus. If there are two masters on the same

I2C bus, there is an arbitration procedure if both try to take control of the bus at the same time by generating a START

condition at the same time. Once a master (for example, a microcontroller) has control of the bus, no other master can

take control until the first master sends a STOP condition and places the bus in an idle state.

Arbitration takes place on the SDA line, while the SCL line is set to 1. The master, which transmits a one while the other

master transmits zero, loses arbitration and turns off its data output stage. The master that lost arbitration can continue

to generate clocks until the end of the byte transfer. If both masters address the same slave device, the arbitration

Upon detecting that it has lost arbitration to another master, the DW_apb_i2c stops generating SCL by disabling the output

driver. Figure 85 illustrates the timing of two masters arbitrating on the bus.

Figure 85. Multiple

Master Arbitration

Control of the bus is determined by address or master code and data sent by competing masters, so there is no central

master nor any order of priority on the bus.

Arbitration is not allowed between the following conditions:

• A RESTART condition and a data bit
• A STOP condition and a data bit
• A RESTART condition and a STOP condition

Slaves do not participate in the arbitration process.

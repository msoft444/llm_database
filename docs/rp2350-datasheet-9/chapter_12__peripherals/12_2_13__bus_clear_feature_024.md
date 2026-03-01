---
source_pdf: rp2350-datasheet-9.pdf
repository: llm_database
chapter: Chapter 12. Peripherals
section: 12.2.13. Bus clear feature
pages: 1003-1004
type: technical_spec
generated_at: 2026-03-01T11:44:31.548839+00:00
---

# 12.2.13. Bus clear feature

12.2.13. Bus clear feature

DW_apb_i2c supports the bus clear feature that provides graceful recovery of data SDA and clock SCL lines during unlikely

events in which either the clock or data line is stuck at LOW.

12.2. I2C
1002

RP2350 Datasheet

12.2.13.1. SDA line is stuck at LOW

In case of SDA line stuck at LOW, the master performs the following actions to recover as shown in Figure 88 and Figure

89:

1. Master sends a maximum of nine clock pulses to recover the bus LOW within those nine clocks.

◦The number of clock pulses will vary with the number of bits that remain to be sent by the slave. As the

maximum number of bits is nine, master sends up to nine clock pluses and allows the slave to recover.

◦The master attempts to assert a Logic 1 on the SDA line and check whether SDA is recovered. If the SDA is not

recovered, it will continue to send a maximum of nine SCL clocks.

2. If SDA line is recovered within nine clock pulses, the master will send STOP to release the bus.

3. If SDA line is not recovered even after the ninth clock pulse, you must hardware reset the system.

![Page 1004 figure](images/fig_p1004.png)

*Figure 88. SDA Recovery with 9 SCL Clocks*

*Figure 89. SDA Recovery with 6 SCL Clocks*

12.2.13.2. SCL line is stuck at LOW

In the unlikely event (due to an electric failure of a circuit) where the clock (SCL) is stuck to LOW, there is no effective

method to overcome this problem. Instead, reset the bus using the hardware reset signal.

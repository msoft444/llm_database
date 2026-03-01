---
source_pdf: rp2350-datasheet-8.pdf
repository: llm_database
chapter: Chapter 12. Peripherals
section: 12.2.9. Clock synchronisation
pages: 996-997
type: technical_spec
generated_at: 2026-03-01T08:45:43.944040+00:00
---

# 12.2.9. Clock synchronisation

12.2.9. Clock synchronisation

When two or more masters try to transfer information on the bus at the same time, they must arbitrate and synchronize

the SCL clock. All masters generate their own clock to transfer messages. Data is valid only during the high period of SCL

12.2. I2C
995

RP2350 Datasheet

clock. Clock synchronisation is performed using the wired-AND connection to the SCL signal. When the master

transitions the SCL clock to zero, the master starts counting the low time of the SCL clock and transitions the SCL clock

signal to one at the beginning of the next clock period. However, if another master is holding the SCL line to 0, then the

master goes into a HIGH wait state until the SCL clock line transitions to one.

All masters then count off their high time, and the master with the shortest high time transitions the SCL line to zero. The

masters then count out their low time and the one with the longest low time forces the other masters into a HIGH wait

state. Therefore, a synchronized SCL clock is generated, which is illustrated in Figure 86. Optionally, slaves may hold the

SCL line low to slow down the timing on the I2C bus.

![Page 997 figure](images/fig_p0997.png)

Figure 86. Multi-

Master Clock

Synchronisation

SCL LOW transition Resets all CLKs 
to start counting their LOW periods

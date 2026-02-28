---
source_pdf: rp2350-datasheet-1.pdf
repository: llm_database
chapter: Chapter 7. Resets
section: 7.6.2. Brownout detection (BOD)
pages: 510-512
type: technical_spec
generated_at: 2026-02-28T17:22:56.644477+00:00
---

# 7.6.2. Brownout detection (BOD)

7.6.1. Power-on reset (POR)
The power-on reset block ensures the chip starts up cleanly when power is first applied. It accomplishes this by holding
the chip in reset until the digital core supply (DVDD) reaches a voltage high enough to reliably power the chip’s core logic.
The block holds its por_n output low until DVDD exceeds the power-on reset threshold (DVDDTH.POR) for a period greater
than the power-on reset assertion delay (tPOR.ASSERT). Once high, por_n remains high even if DVDD subsequently falls below
DVDDTH.POR. The behaviour of por_n when power is applied is shown in Figure 28, “A power-on reset cycle”.
DVDD
por_n
DVDDTH.POR
tPOR.ASSERT
Figure 28. A power-on
reset cycle
DVDDTH.POR is fixed at a nominal 0.957V, which should result in a threshold between 0.924V and 0.99V. The threshold
assumes a nominal DVDD of 1.1V at initial power-on, and por_n may never go high if a lower voltage is used. Once the chip
is out of reset, DVDD can be reduced without por_n going low.
7.6.1.1. Detailed specifications
Table 538. Power-on
Reset Parameters
Parameter
Description
Min
Typ
Max
Units
DVDDTH.POR
power-on reset
threshold
0.924
0.957
0.99
V
tPOR.ASSERT
power-on reset
assertion delay
3
10
μs
7.6.2. Brownout detection (BOD)
The brownout detection block prevents unreliable operation when the digital core supply (DVDD) drops below a safe
operating level. If enabled, the block resets the chip by taking its bor_n output low when DVDD drops below the brownout
detection assertion threshold (DVDDTH.BOD.ASSERT) for a period greater than the brownout detection assertion delay
(tBOD.ASSERT). If DVDD subsequently rises above the brownout detection de-assertion threshold (DVDDTH.BOD.DEASSERT) for a
period greater than the brownout detection de-assertion delay (tBOD.DEASSERT), the block releases reset by taking bor_n
high. A brownout, followed by supply recovery, is shown in Figure 29, “A brownout detection cycle”.
RP2350 Datasheet
7.6. Power-on resets and brownout detection
509


Figure 29. A brownout
detection cycle
7.6.2.1. Detection enable
Brownout detection is always enabled at initial power-on. There is, however, a short delay, the brownout detection
activation delay (tBOD.ACTIVE), between por_n going high and detection becoming active. This is shown in Figure 30,
“Activation of brownout detection at initial power-on and following a brownout event.”.
Figure 30. Activation
of brownout detection
at initial power-on and
following a brownout
event.
Once the chip is out of reset, detection can be disabled under software control. This saves a small amount of power. If
detection is subsequently re-enabled, there will be another short delay, the brownout detection enable delay (tBOD.ENABLE),
before it becomes active again. This is shown in Figure 31, “Disabling and enabling brownout detection”.
Detection is disabled by writing a 0 to the EN field in the BOD register and is re-enabled by writing a 1 to the same field. The
block’s bod_n output is high when detection is disabled.
EN
tBOD.ENABLE
detection 
inactive
1
0
1
detection 
inactive
detection 
active
Figure 31. Disabling
and enabling brownout
detection
Detection is re-enabled if the BOD register is reset, as this sets the register’s EN field to 1. Again, detection will become
RP2350 Datasheet
7.6. Power-on resets and brownout detection
510


active after a delay equal to the brownout detection enable delay (tBOD.ENABLE).
NOTE
If the BOD register is reset by a power-on or brownout-initiated reset, the delay between the register being reset and
brownout detection becoming active will be equal to the brownout detection activation delay (tBOD.ACTIVE). The delay
will be equal to the brownout detection enable delay (tBOD.ENABLE) for all other reset sources.
7.6.2.2. Adjusting the detection threshold
The brownout detection threshold (DVDDTH.BOD) has a nominal value of 0.946V at initial power-on or after a reset event.
This should result in a detection threshold between 0.913V and 0.979V. Once out of reset, the threshold can be adjusted
under software control. The new detection threshold will take effect after the brownout detection programming delay
((tBOD.PROG). An example of this is shown in Figure 32, “Adjusting the brownout detection threshold”.
The threshold is adjusted by writing to the VSEL field in the BOD register. See the BOD register description for details.
NOTE
The nominal supply voltage for DVDD is 1.1 V. You should not increase the brownout detection threshold above the
nominal supply voltage.
VSEL
tBOD.PROG
threshold 
0.86V
1001
0111
threshold 
0.774V
Figure 32. Adjusting
the brownout
detection threshold
7.6.2.3. Detailed specifications
Table 539. Brownout
Detection Parameters
Parameter
Description
Min
Typ
Max
Units
DVDDTH.BOD.ASSERT
brownout
detection
assertion
threshold
96.5
100
103.5
% of selected
threshold voltage
DVDDTH.BOD.DEASSERT
brownout
detection de-
assertion
threshold
97.4
101
105
% of selected
threshold voltage
tBOD.ACTIVE
brownout
detection
activation delay
55
80
μs
tBOD.ASSERT
brownout
detection
assertion delay
3
10
μs
RP2350 Datasheet
7.6. Power-on resets and brownout detection
511


## Images

![img_p0511_00.png](images/img_p0511_00.png)

![img_p0511_01.png](images/img_p0511_01.png)


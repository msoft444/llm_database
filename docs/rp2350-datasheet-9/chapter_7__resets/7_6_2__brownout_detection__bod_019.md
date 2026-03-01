---
source_pdf: rp2350-datasheet-9.pdf
repository: llm_database
chapter: Chapter 7. Resets
section: 7.6.2. Brownout detection (BOD)
pages: 510-513
type: technical_spec
generated_at: 2026-03-01T11:44:31.548839+00:00
---

# 7.6.2. Brownout detection (BOD)

7.6.2. Brownout detection (BOD)

The brownout detection block prevents unreliable operation when the digital core supply (DVDD) drops below a safe

operating level. If enabled, the block resets the chip by taking its bor_n output low when DVDD drops below the brownout

detection assertion threshold (DVDDTH.BOD.ASSERT) for a period greater than the brownout detection assertion delay

(tBOD.ASSERT). If DVDD subsequently rises above the brownout detection de-assertion threshold (DVDDTH.BOD.DEASSERT) for a

period greater than the brownout detection de-assertion delay (tBOD.DEASSERT), the block releases reset by taking bor_n

high. A brownout, followed by supply recovery, is shown in Figure 29, “A brownout detection cycle”.

7.6. Power-on resets and brownout detection
509

RP2350 Datasheet

*Figure 29. A brownout detection cycle*

7.6.2.1. Detection enable

Brownout detection is always enabled at initial power-on. There is, however, a short delay, the brownout detection

activation delay (tBOD.ACTIVE), between por_n going high and detection becoming active. This is shown in Figure 30,

“Activation of brownout detection at initial power-on and following a brownout event.”.

*Figure 30. Activation of brownout detection at initial power-on and following a brownout event.*

Once the chip is out of reset, detection can be disabled under software control. This saves a small amount of power. If

detection is subsequently re-enabled, there will be another short delay, the brownout detection enable delay (tBOD.ENABLE),

before it becomes active again. This is shown in Figure 31, “Disabling and enabling brownout detection”.

Detection is disabled by writing a 0 to the EN field in the BOD register and is re-enabled by writing a 1 to the same field. The

block’s bod_n output is high when detection is disabled.

![Page 511 figure](images/fig_p0511.png)

*Figure 31. Disabling and enabling brownout detection*

Detection is re-enabled if the BOD register is reset, as this sets the register’s EN field to 1. Again, detection will become

7.6. Power-on resets and brownout detection
510

RP2350 Datasheet

active after a delay equal to the brownout detection enable delay (tBOD.ENABLE).

NOTE

![Page 512 figure](images/fig_p0512.png)

If the BOD register is reset by a power-on or brownout-initiated reset, the delay between the register being reset and

brownout detection becoming active will be equal to the brownout detection activation delay (tBOD.ACTIVE). The delay

will be equal to the brownout detection enable delay (tBOD.ENABLE) for all other reset sources.

7.6.2.2. Adjusting the detection threshold

The brownout detection threshold (DVDDTH.BOD) has a nominal value of 0.946V at initial power-on or after a reset event.

This should result in a detection threshold between 0.913V and 0.979V. Once out of reset, the threshold can be adjusted

under software control. The new detection threshold will take effect after the brownout detection programming delay

((tBOD.PROG). An example of this is shown in Figure 32, “Adjusting the brownout detection threshold”.

The threshold is adjusted by writing to the VSEL field in the BOD register. See the BOD register description for details.

The nominal supply voltage for DVDD is 1.1 V. You should not increase the brownout detection threshold above the

*Figure 32. Adjusting the brownout detection threshold*

7.6.2.3. Detailed specifications

| Parameter | Description | Min | Typ | Max | Units |
| --- | --- | --- | --- | --- | --- |
| DVDD TH.BOD.ASSERT | brownout detection assertion threshold | 96.5 | 100 | 103.5 | % of selected threshold voltage |
| DVDD TH.BOD.DEASSERT | brownout detection de- assertion threshold | 97.4 | 101 | 105 | % of selected threshold voltage |
| t BOD.ACTIVE | brownout detection activation delay |  | 55 | 80 | μs |
| t BOD.ASSERT | brownout detection assertion delay |  | 3 | 10 | μs |
| t BOD.DEASSERT | brownout detection de- assertion delay |  | 55 | 80 | μs |
| t BOD.ENABLE | brownout detection enable delay |  | 35 | 55 | μs |
| t BOD.PROG | brownout detection programming delay |  | 20 | 30 | μs |

*Table 539. Brownout*

7.6. Power-on resets and brownout detection
511

RP2350 Datasheet

## Embedded Images

![img_p0511_00.png](images/img_p0511_00.png)

![img_p0511_01.png](images/img_p0511_01.png)


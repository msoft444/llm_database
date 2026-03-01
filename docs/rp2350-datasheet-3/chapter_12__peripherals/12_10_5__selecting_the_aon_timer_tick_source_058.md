---
source_pdf: rp2350-datasheet-3.pdf
repository: llm_database
chapter: Chapter 12. Peripherals
section: 12.10.5. Selecting the AON Timer tick source
pages: 1200-1201
type: technical_spec
generated_at: 2026-03-01T02:30:35.833805+00:00
---

# 12.10.5. Selecting the AON Timer tick source

![Page 1200 figure](images/fig_p1200.png)

RP2350 Datasheet

• ALARM_TIME_31TO16
• ALARM_TIME_15TO0

To avoid false alarms, disable the alarm before setting the alarm time.

To enable the alarm, use TIMER.ALARM_ENAB.

When the alarm fires, the AON Timer sets the alarm status flag TIMER.ALARM.

To clear the alarm status flag, write a 1 to the alarm status flag.

To configure the alarm to trigger a power-up, set TIMER.PWRUP_ON_ALARM. This feature is not available to Non-secure

code.

The alarm can be configured to trigger an interrupt. The interrupt is handled in the standard way using the following

register fields:

• INTR.TIMER - raw interrupt
• INTE.TIMER - interrupt enable
• INTF.TIMER - force interrupt
• INTS.TIMER - interrupt status

12.10.5. Selecting the AON Timer tick source

The AON Timer indicates the current configuration with read-only flags. Table 1252 provides a list of sources supported

by the 1kHz AON Timer tick.

| Tick source | Read-only flag |
| --- | --- |
| LPOSC clock division | TIMER.USING_LPOSC |
| XOSC clock division | TIMER.USING_XOSC |
| external 1kHz tick | TIMER.USING_GPIO_1KHZ |

Table 1252. AON

NOTE

The LPOSC clock can be substituted by an external 32kHz clock.

12.10.5.1. Using LPOSC as the AON Timer tick source

LPOSC is the default source and can be used in all power modes. It nominally runs at 32.768kHz and can only be tuned

to 1% accuracy. The AON Timer derives the 1ms tick from the LPOSC using a 6.16 bit fractional divider whose divisor is

initialised to 32.768. The divisor can be modified to achieve greater accuracy. Because the LPOSC frequency varies with

supply voltage and temperature, accuracy is limited unless supply voltage and temperature are stable. To modify the

divisor, write to the following registers:

• LPOSC_FREQ_KHZ_INT (default value: 32)
• LPOSC_FREQ_KHZ_FRAC (default value: 0.768)

These registers should only be written when TIMER.RUN = 0 or TIMER.USING_LPOSC = 0.

If the tick source is not LPOSC, you can switch it back to LPOSC by writing a 1 to TIMER.USE_LPOSC. It is not necessary

to stop the AON Timer to do this. The newly selected tick will be synchronised to the current tick, so the operation may

take up to 1 tick cycle (1ms in normal operation). When the operation is complete, TIMER.USE_LPOSC will self-clear and

TIMER.USING_LPOSC will be set. Due to sampling, a small error of up to 2 periods of the newly selected clock will be

subtracted from the time. When switching to LPOSC at 32kHz, an error of up to 62μs will be subtracted.

12.10. Always-on timer
1199

RP2350 Datasheet

12.10.5.2. Using an external clock in place of LPOSC

If LPOSC isn’t sufficiently accurate, an external 32.768kHz clock can be used. This will be multiplexed onto the internal

low-power clock and will therefore drive all components that are driven by that clock, including the power sequencer

components. The external clock can be used in all power modes. When an external clock is in use, you can stop the

LPOSC (see Section 8.4).

To select an external 32kHz clock:

1. Configure the GPIO source as described in Section 12.10.7.

2. Switch to the external LPOSC by setting EXT_TIME_REF.DRIVE_LPCK. This register should only be written when

TIMER.RUN = 0 and the power sequencer is inactive. You can only write to this register from Secure code.

The external 32kHz clock replaces the clock from LPOSC. Therefore the same registers are used for AON Timer

configuration (see Section 12.10.5.1):

• TIMER.USE_LPOSC
• TIMER.USING_LPOSC
• LPOSC_FREQ_KHZ_INT
• LPOSC_FREQ_KHZ_FRAC

12.10.5.3. Using the XOSC as the AON Timer tick source

The XOSC clock is provided via the reference clock (clk_ref). The user must ensure the reference clock is being driven

from the XOSC before selecting it as the source of the AON Timer tick. This is the normal configuration following boot.

To check, look for CLK_REF_SELECTED = 0x4. The reference clock may be a divided version of the XOSC. The divisor

defaults to 1 and can be read from CLK_REF_DIV.INT. If the chip is operated with a faster XOSC, the clock sent to the

AON Timer must not exceed 29MHz.

The AON Timer derives the 1ms tick from the XOSC using a 16.16 bit fractional divider whose divisor is initialised to

12000.0. This assumes a 12MHz crystal is used and the reference clock divisor is 1. If that is not the case, the divisor in

the AON Timer can be modified by writing to the following registers:

• XOSC_FREQ_KHZ_INT (default value: 12000)
• XOSC_FREQ_KHZ_FRAC (default value: 0)

These registers should only be written when TIMER.RUN = 0 or TIMER.USING_XOSC = 0.

To select the XOSC as the AON Timer tick source, write a 1 to TIMER.USE_XOSC. It is not necessary to stop the AON

Timer to do this. The newly selected tick will be synchronised to the current tick, so the operation may take up to 1 tick

cycle (1ms in normal operation). When the operation is complete TIMER.USE_XOSC will self-clear and

TIMER.USING_XOSC will be set. Due to sampling, a small error of up to 2 periods of the newly selected clock will be

subtracted from the time. When switching to XOSC at 12MHz an error of up to 167ns will be subtracted.

When the chip core is powered down the XOSC will stop. If TIMER.USING_XOSC is set, the power-down sequencer

automatically reverts to TIMER.USING_LPOSC before the XOSC stops.

12.10.5.4. Using an external 1ms tick source

To select an external 1ms tick source, configure the GPIO source as described in Section 12.10.7. Then, write a 1 to

TIMER.USE_GPIO_1KHZ. It is not necessary to stop the AON Timer to do this, however the newly selected tick will not be

synchronised to the current tick, so the operation so the operation will advance the time by up to 1ms. If using an

external 1ms tick it is recommended to set the time after selecting the source. When the operation is complete

TIMER.USE_GPIO_1KHZ will self-clear and TIMER.USING_GPIO_1KHZ will be set.

The tick is triggered from the falling edge of the selected GPIO. For correct sampling, the GPIO pulse width and interval

must both be greater than the period of LPOSC (>31us). This limits the maximum frequency of the external tick to

12.10. Always-on timer
1200

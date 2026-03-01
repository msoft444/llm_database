---
source_pdf: rp2350-datasheet-5.pdf
repository: llm_database
chapter: Chapter 12. Peripherals
section: 12.10.4. Using the alarm
pages: 1199-1200
type: technical_spec
generated_at: 2026-03-01T04:13:03.328193+00:00
---

# 12.10.4. Using the alarm

12.10.4. Using the alarm

To set the alarm time, use the following 4 × 16-bit registers:

• ALARM_TIME_63TO48
• ALARM_TIME_47TO32

12.10. Always-on timer
1198

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

![Page 1200 figure](images/fig_p1200.png)

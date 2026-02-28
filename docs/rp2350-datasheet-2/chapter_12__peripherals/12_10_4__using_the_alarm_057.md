---
source_pdf: rp2350-datasheet-2.pdf
repository: llm_database
chapter: Chapter 12. Peripherals
section: 12.10.4. Using the alarm
pages: 1199-1199
type: technical_spec
generated_at: 2026-02-28T17:43:10.557178+00:00
---

# 12.10.4. Using the alarm

RP2350 Datasheet

unpowered, then a 32kHz clock or a 1ms tick can be supplied from an external source. Alternatively, the AON Timer can

be synchronised to an external 1Hz source.

The AON Timer is integrated with the power manager (POWMAN) and shares the POWMAN register block. Writes are

limited to 16 bits because a key (0x5afe) is required in the top 16 bits to prevent erroneous writes from locking up the

chip. Most AON Timer registers can be enabled for write by Non-secure software, unlike other POWMAN registers.

However, the registers used to select an external clock, select an external tick source, and enable power-up on alarm

can only be written by Secure software.

12.10.2. Changes from RP2040

The RP2040 Real Time Clock (RTC) is not used in RP2350. Instead, RP2350 has a timer in the Always-On power domain

which is used for scheduling power-up events and can also be used as a real-time counter. The AON Timer works

differently from the RP2040 RTC. It counts milliseconds to 64 bits and this value can be used to calculate the date and

time in software if required.

12.10.3. Accessing the AON Timer

To start and stop the AON Timer, write to TIMER.RUN.

To read the current 64-bit AON Timer value, use the following 2 × 32-bit read-only registers:

• READ_TIME_UPPER
• READ_TIME_LOWER

Because the AON Timer can increment during a read, use the following procedure to protect against erroneous reads:

1. Read READ_TIME_UPPER

2. Read READ_TIME_LOWER

3. Read READ_TIME_UPPER

4. If the READ_TIME_UPPER value changes between steps 1 and 3, repeat the whole procedure

When used as a real time clock, the 64-bit time value is set using 4 × 16-bit registers. These registers can only be written

when the AON Timer is stopped by writing a 0 to TIMER.RUN:

• SET_TIME_63TO48
• SET_TIME_47TO32
• SET_TIME_31TO16
• SET_TIME_15TO0

These registers cannot be used to read the time value.

When used as an interval timer, write a 1 to TIMER.CLEAR to clear the timer value. It is not necessary to stop the AON

Timer to do this. The TIMER.CLEAR register is self-clearing: it returns to 0 when the operation completes. This allows

easy implementation of an alarm that wakes the chip or generates an interrupt at regular intervals.

12.10.4. Using the alarm

To set the alarm time, use the following 4 × 16-bit registers:

• ALARM_TIME_63TO48
• ALARM_TIME_47TO32

12.10. Always-on timer
1198

---
source_pdf: rp2350-datasheet-6.pdf
repository: llm_database
chapter: Chapter 12. Peripherals
section: 12.10.3. Accessing the AON Timer
pages: 1199-1199
type: technical_spec
generated_at: 2026-03-01T05:39:21.783275+00:00
---

# 12.10.3. Accessing the AON Timer

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

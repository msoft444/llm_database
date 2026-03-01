---
source_pdf: rp2350-datasheet-8.pdf
repository: llm_database
chapter: Chapter 8. Clocks
section: 8.2.4. Startup delay
pages: 557-558
type: technical_spec
generated_at: 2026-03-01T08:45:43.944040+00:00
---

# 8.2.4. Startup delay

8.2.4. Startup delay

The STARTUP_DELAY register specifies how many clock cycles must be seen from the crystal before it can be used. This is

specified in multiples of 256. The SDK xosc_init function sets this value. The 1 ms default is sufficient for the RP2350

reference design (see Hardware design with RP2350, Minimal Design Example) which runs the XOSC at 12 MHz. When

the timer expires, the STATUS_STABLE flag will be set to indicate the XOSC output can be used.

Before starting the XOSC the programmer must ensure the STARTUP_DELAY register is correctly configured. The required

value can be calculated by:

So with a 12 MHz crystal and a 1 ms wait time, the calculation is:

8.2. Crystal oscillator (XOSC)
556

RP2350 Datasheet

NOTE

The value is rounded up to the nearest integer, so the wait time will be just over 1 ms.

## Embedded Images

![img_p0557_00.png](images/img_p0557_00.png)

![img_p0557_01.png](images/img_p0557_01.png)


---
source_pdf: rp2350-datasheet-7.pdf
repository: llm_database
chapter: Chapter 8. Clocks
section: 8.2.2. Changes from RP2040
pages: 557-557
type: technical_spec
generated_at: 2026-03-01T06:07:53.318745+00:00
---

# 8.2.2. Changes from RP2040

8.2.2. Changes from RP2040

• Maximum crystal frequency increased from 15 MHz to 50 MHz, when appropriate range is selected in

CTRL.FREQ_RANGE

NOTE

The above change applies when using the XOSC as a crystal oscillator, with a crystal connected between the XIN and

XOUT pins. When using the XOSC XIN pin as a CMOS clock input from an external oscillator, the maximum is always

50 MHz. You do not have to configure CTRL.FREQ_RANGE for the CMOS input case. The CMOS input behaviour is

the same as RP2040.

NOTE

The maximum clk_ref frequency is 25 MHz. If you use a >25 MHz crystal as the source of clk_ref, you must divide

the XOSC output using the clk_ref divider.

## Embedded Images

![img_p0557_00.png](images/img_p0557_00.png)

![img_p0557_01.png](images/img_p0557_01.png)


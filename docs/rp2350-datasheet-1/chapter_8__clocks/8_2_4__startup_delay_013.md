---
source_pdf: rp2350-datasheet-1.pdf
repository: llm_database
chapter: Chapter 8. Clocks
section: 8.2.4. Startup delay
pages: 557-557
type: technical_spec
generated_at: 2026-02-28T17:22:56.644477+00:00
---

# 8.2.4. Startup delay

ensure stability.
The crystal oscillator is powered from the VDDIO voltage. As a result, the Abracon crystal and that particular damping
resistor are tuned for 3.3V operation. If you use a different IO voltage, you will need to re-tune.
Any changes to crystal parameters risk instability across any components connected to the crystal circuit.
If you can’t source the recommended crystal directly from Abracon or a reseller, contact applications@raspberrypi.com.
Raspberry Pi Pico 2 has been specifically tuned for the specifications of the Abracon ABM8-272-T3 crystal. For an
example of how to use a crystal with RP2350, see the Raspberry Pi Pico 2 board schematic in Appendix B of Raspberry
Pi Pico 2 Datasheet and the Raspberry Pi Pico 2 design files.
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
8.2.3. Usage
The XOSC is disabled on chip startup and RP2350 boots using the Ring Oscillator (ROSC). To start the XOSC, the
programmer must set the CTRL_ENABLE register. The XOSC is not immediately usable because it takes time for the
oscillations to build to sufficient amplitude. This time will be dependent on the chosen crystal but will be of the order of
a few milliseconds. The XOSC incorporates a timer controlled by the STARTUP_DELAY register to automatically manage this,
which sets a flag (STATUS_STABLE) when the XOSC clock is usable.
8.2.4. Startup delay
The STARTUP_DELAY register specifies how many clock cycles must be seen from the crystal before it can be used. This is
specified in multiples of 256. The SDK xosc_init function sets this value. The 1 ms default is sufficient for the RP2350
reference design (see Hardware design with RP2350, Minimal Design Example) which runs the XOSC at 12 MHz. When
the timer expires, the STATUS_STABLE flag will be set to indicate the XOSC output can be used.
Before starting the XOSC the programmer must ensure the STARTUP_DELAY register is correctly configured. The required
value can be calculated by:
So with a 12 MHz crystal and a 1 ms wait time, the calculation is:
RP2350 Datasheet
8.2. Crystal oscillator (XOSC)
556


## Images

![img_p0557_00.png](images/img_p0557_00.png)

![img_p0557_01.png](images/img_p0557_01.png)


---
source_pdf: rp2350-datasheet-2.pdf
repository: llm_database
chapter: Chapter 8. Clocks
section: 8.1.2. Clock sources
pages: 515-518
type: technical_spec
generated_at: 2026-02-28T17:43:10.557178+00:00
---

# 8.1.2. Clock sources

RP2350 Datasheet

The clock generators select from the clock sources and optionally divide the selected clock before outputting through

enable logic that provides automatic clock gating in sleep mode (see Section 8.1.3.5.2, “System sleep mode”).

An on-chip frequency counter facilitates debugging of the clock setup and also allows measurement of the frequencies

of LPOSC, ROSC and external clocks. If the system clock stops accidentally, the on-chip resus (short for resuscitate)

component restarts it from a known good clock. This allows the software debugger to access registers and debug the

problem.

When the switched-core is powered, the power manager clock automatically switches to the reference clock (clk_ref).

The user can optionally switch the AON Timer tick, though we recommend waiting until clk_ref is running from the

XOSC, because the ROSC frequency is imprecise.

You can substitute the clock sources with up to 2 GPIO clock inputs. This helps avoid adding a second crystal into

systems that already have an accurate clock source and enables replacement of the ROSC and LPOSC with more

accurate external sources.

You can also output up to 4 generated clocks to GPIOs at up to 50MHz. This enables you to supply clocks to external

devices, reducing the need for additional clock components that consume power and board area.

8.1.1. Changes between RP2350 revisions

RP2350 A3 changes the reset values of:

• CLK_SYS_CTRL.SRC from 0 to 1 (select AUX source).
• CLK_SYS_CTRL.AUXSRC from 0 to 2 (select ROSC as AUX source).

See Hardware changes for information about related changes made to the ROSC configuration at reset. See Bootrom

changes for related changes made in the A3 boot ROM.

8.1.2. Clock sources

RP2350 can use a variety of clock sources. This flexibility allows the user to optimise the clock setup for performance,

cost, board area and power consumption. RP2350 supports the following potential clock sources:

• On-chip 32kHz low-power oscillator (Section 8.4, “Low Power oscillator (LPOSC)”)
• On-chip ring oscillator (Section 8.3, “Ring oscillator (ROSC)”)
• Crystal oscillator (Section 8.2, “Crystal oscillator (XOSC)”)
• External clocks from GPIOs (Section 8.1.6.4, “Configuring a GPIO input clock”) and PLLs (Section 8.6, “PLL”)

The list of clock sources is different per clock generator and can be found as enumerated values in the CTRL register.

See CLK_SYS_CTRL as an example.

8.1.2.1. Low-power oscillator

The on-chip 32kHz low-power oscillator (Section 8.4, “Low Power oscillator (LPOSC)”) requires no external components.

It starts automatically when the always-on domain is powered, providing a clock for the power manager and a tick for

the Always-on Timer (AON Timer) when the switched-core power domain is powered off.

The LPOSC can be tuned to 1% accuracy, and the divider in the AON Timer tick generator can further tune the 1ms tick.

However, the LPOSC frequency varies with voltage and temperature, so fine-tuning is only useful in systems with stable

voltage and temperature.

When the switched-core is powered, the LPOSC clock can drive the reference clock (clk_ref), which in turn can drive the

system clock (clk_sys). This allows another low-power mode where the processors remain powered but, unlike the

SLEEP and DORMANT modes, clocks are running. The LPOSC clock can also be sent to the frequency counter for

calibration or output to a GPIO.

8.1. Overview
514

RP2350 Datasheet

8.1.2.2. Ring oscillator

The on-chip ring oscillator (Section 8.3, “Ring oscillator (ROSC)”) requires no external components. It starts

automatically when the switched-core domain is powered and is used to clock the chip during the initial boot stages.

During boot, the ROSC runs at a nominal 11MHz, but varies with PVT (Process, Voltage, and Temperature). The ROSC

frequency is guaranteed to be in the range 4.6MHz to 19.6MHz.

For low-cost applications where frequency accuracy is unimportant, the chip can continue to run from the ROSC. If your

application requires greater performance, the frequency can be increased by programming the registers as described in

Section 8.3, “Ring oscillator (ROSC)”. Because the frequency varies with PVT (Process, Voltage, and Temperature), the

user must take care to avoid exceeding the maximum frequencies described in the clock generators section. For

information about reducing this variation when running the ROSC at frequencies close to the maximum, see Section

8.1.2.2.1, “Mitigate ROSC frequency variation due to process”. Alternatively, use an external clock or the XOSC to

provide a stable reference clock and use the PLLs to generate higher frequencies. However, this approach requires

external components, which will cost board area and increase power consumption.

When using an external clock or the XOSC, you can stop the ROSC to save power. Before stopping the ROSC, you must

switch the reference clock generator and the system clock generator to an alternate source.

The ROSC is unpowered when the switched-core domain is powered down, but starts immediately when the switched-

core powers up. It is not affected by sleep mode. To save power, reduce the frequency before entering sleep mode.

When entering DORMANT mode, the ROSC is automatically stopped. When exiting DORMANT mode, the ROSC restarts

in the same configuration. If you drive clocks at close to their maximum frequencies with the ROSC, drop the frequency

before entering SLEEP or DORMANT mode. This allows for frequency variation due to changes in environmental

conditions during SLEEP or DORMANT mode.

To use ROSC clock externally, output it to a GPIO pin using one of the clk_gpclk0-3 generators.

The following sections describe techniques for mitigating PVT variation of the ROSC frequency. They also provide some

interesting design challenges for use in teaching both the effects of PVT and writing software to control real time

functions.


TIP

Because the ROSC frequency varies with PVT (Process, Voltage, and Temperature), you can use the ROSC frequency

to measure any one of the three PVT variables as long as you know the other two variables.

8.1.2.2.1. Mitigate ROSC frequency variation due to process

Process varies for the following reasons:

• Chips leave the factory with a spread of process parameters. This causes variation in the ROSC frequency across

chips.
• Process parameters vary slightly as the chip ages. This is only observable over many thousands of hours of

operation.

To mitigate process variation, the user can characterise individual chips and program the ROSC frequency accordingly.

This is an adequate solution for small numbers of chips, but does not scale well to volume production. For high-volume

applications, consider using automatic mitigation.

8.1.2.2.2. Mitigate ROSC frequency variation due to voltage

Supply voltage varies for the following reasons:

• The power supply itself can vary.
• As chip activity varies, on-chip IR varies.

To mitigate voltage variation, calibrate for the minimum performance target of your application, then adjust the ROSC

8.1. Overview
515

RP2350 Datasheet

frequency to always exceed that minimum.

8.1.2.2.3. Mitigate ROSC frequency variation due to temperature

Temperature varies for the following reasons:

• The ambient temperature can vary.
• The chip temperature varies as chip activity varies due to self-heating.

To mitigate temperature variations, stabilise the temperature. You can use a temperature controlled environment,

passive cooling, or active cooling. Alternatively, track the temperature using the on-chip temperature sensor and adjust

the ROSC frequency so it remains within the required bounds.

8.1.2.2.4. Automatic mitigation of ROSC frequency variation due to PVT

Techniques for automatic ROSC frequency control avoid the need to calibrate individual chips, but require periodic

access to a clock reference or to a time reference.

If a clock reference is available, you can use it to periodically measure the ROSC frequency and adjust accordingly. The

on-chip XOSC is one potential clock reference. You can even run the XOSC intermittently to save power for very low-

power application where it is too costly to run the XOSC continuously or use the PLLs to achieve high frequencies.

If a time reference is available, you can clock the on-chip AON Timer from the ROSC and periodically compare it against

the time reference, adjusting the ROSC frequency as necessary. Using these techniques, the ROSC frequency still drifts

due to voltage and temperature variation. As a result, you should also implement mitigations for voltage and

temperature to ensure that variations do not allow the ROSC frequency to drift out of the acceptable range.

8.1.2.2.5. Automatic overclocking using the ROSC

The datasheet maximum frequencies for any digital device are quoted for worst case PVT. Most chips in most normal

environments can run significantly faster than the quoted maximum, and therefore support overclocking. When RP2350

runs from the ROSC, PVT affects both both the ROSC and the digital components. As the ROSC gets faster, the

processors can also run faster. This means the user can overclock from the ROSC, then rely on the ROSC frequency

tracking with PVT variations. The tracking of ROSC frequency and the processor capability is not perfect, and currently

there is insufficient data to specify a safe ROSC setting for this mode of operation, so some experimentation is

required.

This mode of operation maximises processor performance, but causes variations in the time taken to complete a task.

Only use overclocking for applications where this variation is acceptable. If your application uses frequency sensitive

interfaces such as USB or UART, you must use the XOSC and PLL to provide a precise clock for those components.

8.1.2.3. Crystal oscillator

The Crystal Oscillator (Section 8.2, “Crystal oscillator (XOSC)”) provides a precise, stable clock reference and should be

used where accurate timing is required and no suitable external clocks are available. The XOSC requires an external

crystal component. The external crystal determines the frequency. RP2350 supports 1MHz to 50MHz crystals and the

RP2350 reference design (see Hardware design with RP2350, Minimal Design Example) uses a 12MHz crystal. Using

the XOSC and the PLLs, you can run on-chip components at their maximum frequencies. Appropriate margin is built into

the design to tolerate up to 1000ppm variation in the XOSC frequency.

The XOSC is unpowered when the switched-core domain is powered down. It remains inactive when the switched-core

is powered up. If required, you must enable it in software. XOSC startup takes several milliseconds, and software must

wait for the XOSC_STABLE flag to be set before starting the PLLs and changing any clock generators. Before the XOSC

completes startup, output might be non-existent or exhibit very short pulse widths; this will corrupt logic if used. When

XOSC startup is complete, the reference clock (clk_ref) and the system clock (clk_sys) can run from the XOSC. If you

8.1. Overview
516

![Page 518 figure](images/fig_p0518.png)

RP2350 Datasheet

switch the system and reference clocks to run from the XOSC, you can stop the ROSC to save power.

The XOSC is not affected by sleep mode. It automatically stops and restarts in the same configuration when entering

and exiting DORMANT mode.

To use the XOSC clock externally, output it to a GPIO pin using one of the clk_gpclk0-clk_gpclk03 generators. You cannot

take XOSC output directly from the XIN (XI) or XOUT (XO) pins.

8.1.2.4. External clocks

If external clocks exist in the hardware design, you can use them to clock RP2350. You can use clocks individually or in

conjunction with the other (internal or external) clock sources. Use XIN and one of GPIN0-GPIN1 to input external

clocks.

If you drive an external clock into XIN, you don’t need an external crystal. When driving an external clock into XIN, you

must configure the XOSC to pass through the XIN signal. When the switched-core powers down, this configuration will

be lost, but the configuration is unaffected by SLEEP and DORMANT modes. The input is limited to 50MHz, but the on-

chip PLLs can synthesise higher frequencies from the XIN input if required.

GPIN0-GPIN1 can provide system and peripherals clocks, but is limited to 50MHz. This can potentially save power and

allows components on RP2350 to run synchronously with external components, which simplifies data transfer between

chips. If the frequency accuracy of the external clocks is poorer than 1000ppm, the generated clocks should not run at

their maximum frequencies since they could exceed their design margins. Once the external clocks begin to run, the

reference clock (clk_ref) and the system clock (clk_sys) can run from the external clocks and you can stop the ROSC to

save power. When the switched-core powers down, GPIN0-GPIN1 configuration will be lost, but the configuration is

unaffected by SLEEP and DORMANT modes.

To provide a more accurate tick to the AON Timer, use one of the GPIN0-GPIN3 inputs to replace the clock from the

LPOSC. These inputs are limited to 29MHz. GPIN0-GPIN3 configuration is unaffected by switched-core power down,

sleep mode, and DORMANT mode.

8.1.2.5. Relaxation oscillators

If there is no appropriate clock available, but you still want to replace or supplement external clocks with another clock

source, you can construct one or two relaxation oscillators from external passive components. Send the clock source

(GPIN0-GPIN1) to one of the clk_gpclk0-clk_gpclk03 generators, invert it through the GPIO inverter OUTOVER, and connect

back to the clock source input via an RC circuit:

Figure 34. Simple

relaxation oscillator

example

The frequency of clocks generated from relaxation oscillators depend on the delay through the chip and the drive

current from the GPIO output, both of which vary with PVT. The frequency and frequency accuracy depend on the

quality and accuracy of the external components. More elaborate external components such as ceramic resonators, can

improve performance, but also increase cost and complexity. Such an oscillator will not achieve 1000ppm, so they

cannot drive internal clocks at their maximum frequencies. To drive internal clocks at the maximum possible frequency,

use the XOSC.

The configuration of the relaxation oscillators will be lost when the switched-core powers down, but is not affected by

sleep mode or DORMANT mode.

8.1. Overview
517

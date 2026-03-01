---
source_pdf: rp2350-datasheet-3.pdf
repository: llm_database
chapter: Chapter 12. Peripherals
section: 12.10.8. Using a tick faster than 1ms
pages: 1202-1202
type: technical_spec
generated_at: 2026-03-01T02:30:35.833805+00:00
---

# 12.10.8. Using a tick faster than 1ms

RP2350 Datasheet

16kHz.

The external 1ms tick can be used in all power modes.

12.10.6. Synchronising the AON timer to an external 1Hz clock

In applications that use GPS, a 1s tick may be available. This can be used to synchronise the AON Timer and thus

compensate for inaccuracy in the LPOSC frequency. It can be used with any tick source, but there is little to be gained if

the selected source is already reasonably accurate.

If the LPOSC is fast, the ms counter pauses at a 1 second step until the 1s tick is received. If the LPOSC is slow, the 1s

tick causes the ms counter to run very quickly until reaching the 1 second step. This ensures that all ms values are

counted, ensuring that any alarm set to ms precision will fire. A more sophisticated synchronisation method can be

implemented in software.

To use the hardware synchronisation feature, configure the GPIO source as described in Section 12.10.7. Then, enable

the feature by writing a 1 to TIMER.USE_GPIO_1HZ. This can be set at any time, it is not necessary to stop the AON

Timer. When the operation is complete TIMER.USE_GPIO_1HZ will self-clear and TIMER.USING_GPIO_1HZ will be set.

The tick is triggered from the falling edge of the selected GPIO. For correct sampling, the GPIO pulse width and interval

must be greater than the period of LPOSC (>31us).

The external 1s tick can be used in all power modes.

12.10.7. Using an external clock or tick from GPIO

The following features use a GPIO as a clock or a tick:

• external 32kHz clock source
• external 1kHz tick
• external 1Hz tick

Only 4 GPIOs are available for these features. You can only select one, because they share the same GPIO selection

logic. The set of 4 GPIOs differs between package types. The selection is controlled by a 2-bit register field.

The AON Timer uses the following GPIOs:

• EXT_TIME_REF.SOURCE_SEL = 0 → GPIO12
• EXT_TIME_REF.SOURCE_SEL = 1 → GPIO20
• EXT_TIME_REF.SOURCE_SEL = 2 → GPIO14
• EXT_TIME_REF.SOURCE_SEL = 3 → GPIO22

12.10.8. Using a tick faster than 1ms

The tick rate can be increased by scaling the value written to the LPOSC and XOSC frequency registers. For example, if

the frequency value is divided by 4 then the AON Timer will tick 4 times per ms. The minimum value that can be written

to the frequency registers is 2.0, therefore the maximum upscaling using this method with LPOSC is 16, giving a time

resolution of 1/16th of 1 ms (= 62.5us).

As described previously, the external tick is limited to 16kHz, so the maximum upscaling using this method is also 16.

This gives a time resolution of 1/16th of 1 ms (62.5μs).

These limitations can be overcome either by using a faster external clock (see Section 12.10.5.2) or keeping the chip

core powered so the AON Timer is always running from the XOSC. If a faster external clock is used then the power

sequencer timings will also need to be adjusted.

12.10. Always-on timer
1201

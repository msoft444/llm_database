---
source_pdf: rp2350-datasheet-5.pdf
repository: llm_database
chapter: Chapter 12. Peripherals
section: 12.10.6. Synchronising the AON timer to an external 1Hz clock
pages: 1202-1202
type: technical_spec
generated_at: 2026-03-01T04:13:03.328193+00:00
---

# 12.10.6. Synchronising the AON timer to an external 1Hz clock

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

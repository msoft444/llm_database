---
source_pdf: rp2350-datasheet-3.pdf
repository: llm_database
chapter: Chapter 8. Clocks
section: 8.2.6. DORMANT mode
pages: 558-558
type: technical_spec
generated_at: 2026-03-01T02:30:35.833805+00:00
---

# 8.2.6. DORMANT mode

![Page 558 figure](images/fig_p0558.png)

RP2350 Datasheet

NOTE

The value is rounded up to the nearest integer, so the wait time will be just over 1 ms.

8.2.5. XOSC counter

The COUNT register provides a method of managing short software delays. To use this method:

1. Write a value to the COUNT register. The register automatically begins to count down to zero at the XOSC frequency.

2. Poll the register until it reaches zero.

This is preferable to using NOPs in software loops because it is independent of the core clock frequency, the compiler,

and the execution time of the compiled code.

8.2.6. DORMANT mode

In DORMANT mode (see Section 6.5.3, “DORMANT state”), all of the on-chip clocks can be paused to save power. This

is particularly useful in battery-powered applications. RP2350 wakes from DORMANT mode by interrupt: either from an

external event, such as an edge on a GPIO pin, or from the AON Timer. This must be configured before entering

DORMANT mode. To use the AON Timer to trigger a wake from DORMANT mode, it must be clocked from the LPOSC or

from an external source.

To enter DORMANT mode:

1. Switch all internal clocks to be driven from XOSC or ROSC and stop the PLLs.

2. Choose an oscillator (XOSC or ROSC). Write a specific 32-bit value to the DORMANT register of the chosen oscillator to

stop it.

When exiting DORMANT mode, the chosen oscillator will restart. If you chose XOSC, the frequency will be more precise,

but the restart will take more time due to startup delay (>1 ms on the RP2350 reference design (see Hardware design

with RP2350, Minimal Design Example)). If you chose ROSC, the frequency will be less precise, but the start-up time is

very short (approximately 1μs). See Section 6.5.3.1, “Waking from the DORMANT state” for the events which cause the

system to exit DORMANT mode.

NOTE

You must stop the PLLs before entering DORMANT mode.

SDK: https://github.com/raspberrypi/pico-sdk/blob/master/src/rp2_common/hardware_xosc/xosc.c Lines 56 - 63

56 void xosc_dormant(void) {

57     // WARNING: This stops the xosc until woken up by an irq

58     xosc_hw->dormant = XOSC_DORMANT_VALUE_DORMANT;

59     // Wait for it to become stable once woken up

60     while(!(xosc_hw->status & XOSC_STATUS_STABLE_BITS)) {

61         tight_loop_contents();

62     }

63 }

8.2. Crystal oscillator (XOSC)
557

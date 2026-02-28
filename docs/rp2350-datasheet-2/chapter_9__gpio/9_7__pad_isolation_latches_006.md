---
source_pdf: rp2350-datasheet-2.pdf
repository: llm_database
chapter: Chapter 9. GPIO
section: 9.7. Pad isolation latches
pages: 597-597
type: technical_spec
generated_at: 2026-02-28T17:43:10.557178+00:00
---

# 9.7. Pad isolation latches

RP2350 Datasheet

9.6.1. Bus keeper mode

For each pad, only the pull-up or the pull-down resistor can be enabled at any given time. It is impossible to enable both

simultaneously. Instead, if you set both the GPIO0.PDE and GPIO0.PUE bits simultaneously then you enable bus keeper

mode, where the pad is:

• Pulled up when its input is high.
• Pulled down when its input is low.

When the output buffer is disabled, and the pad is not driven by any external source, this mode weakly retains the pad’s

current logical state. The pad does not float to mid-rail.

Bus keeper mode relies on control logic in the switched core domain, so does not function when the core is powered

down. Rather, powering down the core when bus keeper mode is enabled latches the current output controls (pull-up or

pull-down) in the pad isolation latches, as described in Section 9.7.

9.7. Pad isolation latches

RP2350 features extended low-power states that allow all internal logic, with the exception of POWMAN and some

CoreSight debug logic, to fully power down under software control. This includes powering down all peripherals, the IO

muxing, and the pad control registers, which brings with it the risk that pad signals may experience unwanted

transitions when entering and exiting low-power states.

To ensure that pad states are well-defined at all times, all signals passing from the switched core power domain to the

pads pass through isolation latches. In normal operation, the latches are transparent, so the pads are controlled fully by

logic inside the switched core power domain, such as UARTs or the processors. However, when the ISO bit for each pad

is set (e.g. GPIO0.ISO) or the switched core domain is powered down, the control signals currently presented to that pad

are latched until the isolation is disabled. This includes the output enable state, output high/low level, and pull-up/pull-

down resistor enable. The input signal from the pad back into the switched core domain is not isolated.

Consequently, when switched core logic is powered down, all Bank 0 and Bank 1 pads maintain the output state they

held immediately before the power down, unless overridden by always-on logic in POWMAN. When the switched core

power domain powers back up, all the GPIO ISO bits reset to 1, so the pre-power down state continues to be maintained

until user software starts up and clears the ISO bit to indicate it is ready to use the pad again. Pads whose IO muxing

has not yet been set up can be left isolated indefinitely, and will maintain their pre-power down state.

when software has finished setting up the IO muxing for a given pad, and the peripheral that is to be muxed in, the ISO

bit should be cleared. At this point the isolation latches will become transparent again: output signals passing through

the IO muxing block are now reflected in the pad output state, so peripherals can communicate with the outside world.

This process allows the switched core domain to be power cycled without causing any transitions on the pad outputs

that may interfere with the operation of external hardware connected to the pads.

NOTE

Non-SDK applications ported from RP2040 must clear the ISO bit before using a GPIO, as this feature was not

present on RP2040. The SDK automatically clears the ISO bit when gpio_set_function() is called.

The isolation latches themselves are reset by the always-on power domain reset, namely any one of:

• Power-on reset
• Brownout reset
• RUN pin being asserted low
• SW-DP CDBGRSTREQ
• RP-AP rescue reset

The latches reset to the reset value of the signal being isolated. For example, on Bank 0 GPIOs, the input enable control

9.7. Pad isolation latches
596

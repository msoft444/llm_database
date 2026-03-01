---
source_pdf: rp2350-datasheet-7.pdf
repository: llm_database
chapter: Chapter 9. GPIO
section: 9.6.1. Bus keeper mode
pages: 597-597
type: technical_spec
generated_at: 2026-03-01T06:07:53.318745+00:00
---

# 9.6.1. Bus keeper mode

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

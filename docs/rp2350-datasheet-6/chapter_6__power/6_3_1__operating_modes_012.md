---
source_pdf: rp2350-datasheet-6.pdf
repository: llm_database
chapter: Chapter 6. Power
section: 6.3.1. Operating modes
pages: 449-450
type: technical_spec
generated_at: 2026-03-01T05:39:21.783275+00:00
---

# 6.3.1. Operating modes

6.3.1. Operating modes

The regulator has the following three modes of operation.

6.3.1.1. Normal mode

In normal mode, the regulator operates in a switching mode, and can supply up to 200mA. Normal mode is used for P0.x

power states, when the chip’s switched core is powered on. The regulator must be in normal mode before the core

supply current is allowed to exceed 1mA. The regulator starts up in normal mode when its input supplies are first

applied.

6.3.1.2. Low-power mode

In low-power mode, the regulator operates in a linear mode, and can only supply up to 1mA. Low-power mode can be

used for P1.x power states, where the chip’s switched core is powered off. The core supply current must be less than

1mA before the regulator is moved to low-power mode. The regulator’s output voltage is limited to 1.3 V in low-power

mode.

CAUTION

In low-power mode, the output of the regulator is directly connected to DVDD. It isn’t possible to disconnect the

regulator from DVDD in this mode. Don’t put the regulator into low-power mode if DVDD is being powered from an

external supply.

6.3.1.3. High-impedance mode

In high-impedance mode, the regulator is disabled, its power consumption is minimised, and its outputs are set to a

high-impedance state. This mode should only be used if the digital core supply (DVDD) is provided by an external

6.3. Core voltage regulator
448

RP2350 Datasheet

regulator. If the on-chip regulator is supplying DVDD, entering high-impedance mode causes a reset event, returning the

on-chip regulator to Normal mode.

---
source_pdf: rp2350-datasheet-3.pdf
repository: llm_database
chapter: Chapter 6. Power
section: 6.3.1. Operating modes
pages: 449-449
type: technical_spec
generated_at: 2026-03-01T02:30:35.833805+00:00
---

# 6.3.1. Operating modes

RP2350 Datasheet

6.2.3.5. Isolation

When powering down SWCORE, the pad control and data signals are latched and isolated from the IO logic. This avoids

transitions on pads which could potentially corrupt external components. On SWCORE power up, the isolation is not

released automatically. The user releases the isolation by clearing the ISO field of the pad control register (for example

GPIO0.ISO) after the IO logic has been configured.

6.3. Core voltage regulator

RP2350 provides an on-chip voltage regulator for its digital core supply (DVDD). The regulator requires a 2.7 V to 5.5 V

input supply (VREG_VIN), allowing DVDD to be generated directly from a single lithium ion cell, or a USB power supply. A

separate, nominally 3.3 V, low noise supply (VREG_AVDD) is required for the regulator’s analogue control circuits. The

regulator supports both switching and linear modes of regulation, allowing efficient operation at both high and low

loads.

To allow the chip to start up, the regulator is enabled by default, and will power up as soon as its supplies are available.

The regulator starts in switching mode, with a nominal 1.1 V output, but its operating mode and output voltage can be

changed once the chip is out of reset. The output voltage can be set in the range 0.55 V to 3.30 V, and the regulator can

supply up to 200mA.

Although intended for the chip’s digital core supply (DVDD), the regulator can be used for other purposes if DVDD is

powered directly from an external power supply.

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

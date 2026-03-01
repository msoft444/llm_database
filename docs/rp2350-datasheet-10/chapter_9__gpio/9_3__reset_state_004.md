---
source_pdf: rp2350-datasheet-10.pdf
repository: llm_database
chapter: Chapter 9. GPIO
section: 9.3. Reset state
pages: 589-590
type: technical_spec
generated_at: 2026-03-01T12:20:14.494106+00:00
---

# 9.3. Reset state

9.3. Reset state

At first power up, Bank 0 IOs (GPIOs 0 through 29 in the QFN-60 package, and GPIOs 0 through 47 in the QFN-80

package) assume the following state:

• Output buffer is high-impedance
• Input buffer is disabled
• Pulled low
• Isolation latches are set to latched (Section 9.7)

The pad output disable bit (GPIO0.OD) for each pad is clear at reset, but the IO muxing is reset to the null function,

9.2. Changes from RP2040
588

RP2350 Datasheet

which ensures that the output buffer is high-impedance.

IMPORTANT

The pad reset state is different from RP2040, which only disables digital inputs on GPIOs 26 through 29 (as of

version B2) and does not have isolation latches. Applications must enable the pad input (GPIO0.IE = 1) and disable

pad isolation latches (GPIO0.ISO = 0) before using the pads for digital I/O. The gpio_set_function() SDK function

performs these tasks automatically.

Bank 1 IOs have the same reset state as Bank 0 GPIOs, except for the input enable (IE) resetting to 1, and different pull-

up/pull-down states: SCK, SD0 and SD1 are pull-down, but SD2, SD3 and CSn are pull-up.

NOTE

To use a Bank 0 GPIO as a second chip select, you need an external pull-up to ensure the second QSPI device does

not power up with its chip select asserted.

The pads return to the reset state on any of the following:

• A brownout reset
• Asserting the RUN pin low
• Setting SW-DP CDBGRSTREQ via SWD
• Setting RP-AP rescue reset via SWD

If a pad’s isolation latches are in the latched state (Section 9.7) then resetting the PADS and IO registers does not

physically return the pad to its reset state. The isolation latches prevent upstream signals from propagating to the pad.

Clear the ISO bit to allow signals to propagate.

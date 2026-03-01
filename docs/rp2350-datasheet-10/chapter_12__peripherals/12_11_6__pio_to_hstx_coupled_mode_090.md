---
source_pdf: rp2350-datasheet-10.pdf
repository: llm_database
chapter: Chapter 12. Peripherals
section: 12.11.6. PIO-to-HSTX coupled mode
pages: 1209-1209
type: technical_spec
generated_at: 2026-03-01T12:20:14.494106+00:00
---

# 12.11.6. PIO-to-HSTX coupled mode

12.11.6. PIO-to-HSTX coupled mode

HSTX can connect up to 8 PIO pin outputs to the bit crossbar. Only use the bit crossbar when clk_hstx connects directly

to clk_sys ( CLK_HSTX_CTRL.AUXSRC must select clk_sys).

NOTE

Running the two clocks at the same frequency is not sufficient. You must select clk_sys directly.

To enable coupled mode, set CSR.COUPLED_MODE. The COUPLED_SEL field in the same register selects the PIO instance,

0 through 2, to couple to HSTX. When coupled mode is enabled, IO outputs 12 through 19 inclusive on the selected PIO

instance appear at bit crossbar PSEL_N and PSEL_P indices 31:24, replacing the most significant 8 bits of the output shift

register from the point of view of the bit crossbar.

This mode allows PIO programs to make use of the HSTX’s DDR outputs. You can use this mode to drive a clock at the

full system clock rate or to position clock transitions relative to data transitions with half-system-clock-cycle resolution.

The PIO outputs used for couple mode are always bits 19 through 12 of the pin outputs driven from that GPIO,

independent of GPIOBASE. When GPIOBASE is 0, the PIO outputs used for coupled mode are those that would normally

appear on the HSTX pins. When GPIOBASE is 16, this uses the PIO outputs that would appear on GPIOs 28 through 35.

The operation of PIO is not affected in any way by coupled mode being enabled.

Outputs presented through the HSTX coupled mode interface have one additional system clock cycle of delay

compared to those presented directly from PIO to the pads.

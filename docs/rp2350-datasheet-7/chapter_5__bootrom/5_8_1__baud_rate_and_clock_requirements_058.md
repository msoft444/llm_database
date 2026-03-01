---
source_pdf: rp2350-datasheet-7.pdf
repository: llm_database
chapter: Chapter 5. Bootrom
section: 5.8.1. Baud rate and clock requirements
pages: 417-417
type: technical_spec
generated_at: 2026-03-01T06:07:53.318745+00:00
---

# 5.8.1. Baud rate and clock requirements

5.8.1. Baud rate and clock requirements

The nominal baud rate for UART boot is 1 Mbaud, divided from a nominal 48 MHz system clock frequency. UART boot

uses the USB PLL to derive the system clock and UART baud clock, so you must either provide a crystal or drive a stable

clock into the crystal oscillator XIN pad. The host baud rate must match the RP2350 baud rate within 3%.

By default the crystal is assumed to be 12 MHz, but the BOOTSEL_PLL_CFG and BOOTSEL_XOSC_CFG OTP locations

override this to achieve a nominal 48 MHz system clock from any supported crystal. The same OTP configuration is

used for both USB and UART boot.


TIP

You may drive a somewhat faster or slower clock into XIN without any OTP configuration, if you scale your UART

baud rate appropriately. The permissible range is 7.5 to 16 MHz on XIN, limited by the PLL VCO frequency range.

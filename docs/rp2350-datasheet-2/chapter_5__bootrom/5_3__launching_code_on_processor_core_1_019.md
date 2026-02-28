---
source_pdf: rp2350-datasheet-2.pdf
repository: llm_database
chapter: Chapter 5. Bootrom
section: 5.3. Launching code on Processor Core 1
pages: 376-376
type: technical_spec
generated_at: 2026-02-28T17:43:10.557178+00:00
---

# 5.3. Launching code on Processor Core 1

RP2350 Datasheet

sufficiently low impedance to overcome the internal pull-up. A 4.7 kΩ resistance to ground is a good intermediate value

which reliably creates a low input logic level, but will not affect the output levels when RP2350 drives the chip select.

The QSPI SD1 line, which RP2350 initially pulls low, selects which bootloader to enter:

• SD1 remains pulled low: enter USB bootloader
• SD1 driven high: enter UART bootloader

USB boot is a low-friction method for programming an RP2350 from a sophisticated host like a Linux PC. It also directly

exposes more advanced options like OTP programming. See Section 5.5 for the drag-and-drop mass storage interface,

or Section 5.6 for the PICOBOOT vendor interface.

UART boot is a minimal interface for bootstrapping a flashless RP2350 from another microcontroller. UART boot uses

QSPI SD2 for UART TX, and QSPI SD3 for UART RX, at a fixed baud rate of 1 Mbaud. For more details about UART boot,

see Section 5.8.

5.2.8.1. BOOTSEL clock requirements

BOOTSEL mode requires either a crystal attached across the XIN and XOUT pins, or a clock signal from an external

oscillator driven into the XIN pin. See Table 1439 for the electrical specifications of these two XOSC pins.

The bootrom assumes a default XOSC frequency of 12 MHz. It configures the USB PLL to derive a fixed 48 MHz

frequency from the XOSC reference. For USB, this must be a precise frequency. If you use a non-12 MHz crystal, and

intend to use USB boot, program BOOTSEL_PLL_CFG and BOOTSEL_XOSC_CFG in OTP, and then set

BOOT_FLAGS0.ENABLE_BOOTSEL_NON_DEFAULT_PLL_XOSC_CFG. For details about calculating the correct PLL

parameters for your crystal, see Section 8.6.3.

UART boot uses the same PLL configuration as USB boot. However, the permissible range of crystal frequencies under

the default PLL configuration is wider. See Section 5.8.1.

5.2.9. Boot configuration (OTP)

User configuration stored in OTP can be found in Section 13.10, starting at CRIT1.

The main controls for the bootrom are stored in BOOT_FLAGS0 and BOOT_FLAGS1. These are both in page 1 of OTP,

which has the following default permissions on a blank device:

• Read-write for Secure (S)
• Read-write for bootloader (BL)
• Read-only for Non-secure (NS)

Boot key hashes are stored in page 2 of OTP, starting from BOOTKEY0_0. There is space for up to four boot key hashes

in this page. See Section 5.10.1 for an example of how keys can be installed.

5.3. Launching code on Processor Core 1

As described in Section 5.2, after reset, processor core 1 sleeps at start-up, and remains asleep until woken by core 0

via the SIO FIFOs.

If you are using the SDK then you can use the multicore_launch_core1() function to launch code on processor core 1.

However this section describes the procedure to launch code on processor core 1 yourself.

The procedure to start running on processor core 1 involves both cores moving in lockstep through a state machine

coordinated by passing messages over the inter-processor FIFOs. This state machine is designed to be robust enough

to cope with a recently reset processor core 1 which may be anywhere in its boot code, up to and including going to

sleep. As result, the procedure may be performed at any point after processor core 1 has been reset (either by system

5.3. Launching code on Processor Core 1
375

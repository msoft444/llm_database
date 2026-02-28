---
source_pdf: rp2350-datasheet-2.pdf
repository: llm_database
chapter: Chapter 5. Bootrom
section: 5.8.2. UART boot shell protocol
pages: 417-417
type: technical_spec
generated_at: 2026-02-28T17:43:10.557178+00:00
---

# 5.8.2. UART boot shell protocol

![Page 417 figure](images/fig_p0417.png)

RP2350 Datasheet

5.8. UART boot

UART boot is a minimal interface for bootstrapping a flashless RP2350 from a simple host, such as another

microcontroller. It is available by default on a blank device, so it allows RP2350 to be deployed into the field on multi-

device boards without loading firmware or programming OTP bits in advance.

To select UART boot, drive QSPI CSn low (BOOTSEL mode) and drive QSPI SD1 high. The bootrom checks these signals

shortly after device reset is released. UART TX appears on QSPI SD2, and UART RX appears on QSPI SD3.

The UART mode is 8n1: one start bit, eight data bits, no parity, one stop bit. Data within each UART frame is sent and

received LSB-first. The baud rate is fixed at 1 Mbaud.

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

5.8.2. UART boot shell protocol

After the bootrom samples QSPI CSn and SD1, there will be a delay of several milliseconds as the bootrom goes through

some necessary steps such as switching from the ring oscillator to the PLL, and erasing SRAM before releasing it to the

Non-secure UART bootloader.

The UART bootloader signals it is ready to begin by printing the ASCII splash string RP2350. In bytes, this is 0x52, 0x50, 0x32,

0x33, 0x35, 0x30.

Before sending any commands, you must send a special knock sequence to unlock the interface. This is a measure to

avoid transient effects due to noise on GPIOs and ensure the host and device are initially well-synchronised. The

sequence is: 0x56, 0xff, 0x8b, 0xe4. This is the RP2040 UF2 family ID, chosen as a well-known magic number. Any

sequence of bytes ending with this four-byte sequence is detected.

A UART boot shell command is always in the host-to-device direction (RP2350 receives), and consists of a single

command byte, optionally followed by a 32-byte write payload. RP2350 responds with an optional 32-byte read payload

followed by an echo of the command byte. You should wait for the command echo before sending the next command.

The supported commands are:

| Command
(ASCII) | Command
(hex) | Description |
| --- | --- | --- |
| n | 0x6e | No-op. Do nothing, and report back when you’ve done it. Used to ping the interface when
recovering lost synchronisation. Echoes the command byte, 'n'. |
| w | 0x77 | Write a 32-byte payload to the current value of the read/write pointer. Increment the address
pointer by 32. Echoes the command byte, 'w', once all 32 bytes are written to memory. |
| r | 0x72 | Read a 32-byte payload from the current value of the read/write pointer. Increment the
address pointer by 32. Echoes the command byte, 'r', after transmitting the 32-byte read
payload. |

5.8. UART boot
416

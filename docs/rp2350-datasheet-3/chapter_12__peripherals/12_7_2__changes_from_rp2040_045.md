---
source_pdf: rp2350-datasheet-3.pdf
repository: llm_database
chapter: Chapter 12. Peripherals
section: 12.7.2. Changes from RP2040
pages: 1143-1144
type: technical_spec
generated_at: 2026-03-01T02:30:35.833805+00:00
---

# 12.7.2. Changes from RP2040

RP2350 Datasheet

12.7.1.1. Features

The USB controller hardware handles the low level USB protocol. The programmer must configure the controller, provide

data buffers, and consume or provide data buffers in response to events on the bus. The controller interrupts the

processor when it needs attention. The USB controller has 4 kB of dual-port SRAM (DPSRAM) used for configuration

and data buffers.

12.7.1.1.1. Device Mode

In Device Mode, the USB controller has the following characteristics:

• USB 2.0-compatible Full Speed device (12 Mb/s)
• Supports up to 32 endpoints (Endpoints 0 → 15 in both in and out directions)
• Supports Control, Isochronous (ISO), Bulk, and Interrupt endpoint types
• Supports double buffering
• 3840 bytes of usable buffer space in DPSRAM. This is equivalent to 60 × 64-byte buffers

12.7.1.1.2. Host Mode

In Host Mode, the USB controller can:

• communicate with Full Speed (12 Mb/s) devices and Low Speed devices (1.5 Mb/s)
• communicate with multiple devices via a USB hub, including Low Speed devices connected to a Full Speed hub
• poll up to 15 interrupt endpoints in hardware (used by hubs to notify the host of connect/disconnect events, used

by mice to notify the host of movement, etc.)

12.7.1.1.3. USB DPRAM

The USB controller uses 4 kB of dual-port SRAM (DPSRAM) to exchange data and control information with the

controller. This is also referred to as dual-port RAM (DPRAM). One port is accessible from the system bus, clocked by

clk_sys. The other port is accessible from the controller, clocked by clk_usb. The DPRAM is mapped in the system

address space starting from 0x50100000, USBCTRL_DPRAM_BASE.

The USB DPRAM supports 32-bit, 16-bit and 8-bit reads and writes. Writes complete in one cycle. Reads complete in two

cycles.

You can store general user data in USB DPRAM space not required for USB controller operation. When the controller is

disabled, all 4 kB of DPRAM is available. Before accessing the DPRAM, you must take the USB controller out of reset.

Since the USB controller is in the peripheral address space, it is not accessible for processor instruction fetch.

Attempting to fetch instructions from USB DPRAM unconditionally returns a bus error response, no matter the

configuration of the processor SAU/MPU/PMP or the system ACCESSCTRL registers.

As peripheral addresses are marked Exempt in the IDAU (Section 10.2.2), the SAU configuration for this address range

is ignored. Accesses to USB DPRAM are controlled only by the processor MPU/PMP and the ACCESSCTRL USBCTRL

register.

12.7.2. Changes from RP2040

All changes from RP2040 are a superset of the RP2040 features. Existing software for the RP2040 USB controller will

continue to work with one exception: you must clear the MAIN_CTRL.PHY_ISO bit at startup and after power down

events. We recommend leaving the LINESTATE_TUNING register at its reset value. Software should not clear this

register.

12.7. USB
1142

RP2350 Datasheet

12.7.2.1. Errata fixes

RP2350 fixes all RP2040 USB errata. This includes fixes for the following RP2040B0 and B1 errata which are also fixed

by RP2040B2:

• RP2040-E2: USB device endpoint abort is not cleared
• RP2040-E5: USB device fails to exit RESET state on busy USB bus

For more information about RP2040B2, see the RP2040 datasheet.

RP2350 fixes the following RP2040B2 errata, which require software workarounds on RP2040B2:

• RP2040-E3: USB host: interrupt endpoint buffer done flag can be set with incorrect buffer select
• RP2040-E4: USB host writes to upper half of buffer status in single buffered mode
• RP2040-E15: USB Device controller will hang if certain bus errors occur during an IN transfer (see Section

12.7.2.2.4)

12.7.2.2. New features

12.7.2.2.1. General

• The USB PHY DP and DM can be used as regular GPIO pins. See the GPIO muxing Table 646 in Section 9.4..
• A MAIN_CTRL.PHY_ISO control isolates the PHY from the switched core power domain while the switched core

domain is powered down. The isolation control resets to 1, meaning the MAIN_CTRL.PHY_ISO bit needs to be

cleared before the PHY can be used. For more information on isolation, see Chapter 9.
• SIE_CTRL.PULLDOWN_EN defaults to a 1 to match the reset state of isolation latches in the USB PHY. Pulling the

DP and DM pins down by default saves power by preventing them from floating when unused.
• The USB_MUXING.TO_PHY bit defaults to a 1 to match the reset state of isolation latches.
• Added SM_STATE, which exposes the internal state of the controller’s modules.

12.7.2.2.2. Host

• You can now optionally stop a transaction if a NAK is received. This allows the USB host to stop a bulk transaction if

the device is not able to transfer data. Some devices using bulk endpoints, such as a UART, will return NAKs until a

character is received. Stopping the transaction in hardware rather than using software means the host can get a

NAK and guarantee no data has been dropped. RP2350 adds two register bits and an interrupt to support this:

◦The NAK_POLL.STOP_EPX_ON_NAK control, which enables and disables the feature.

◦The NAK_POLL.EPX_STOPPED_ON_NAK status bit, which also has an associated interrupt

INTS.EPX_STOPPED_ON_NAK.
• RP2350 increases inter-packet and turnaround timeouts to accommodate worst-case hub delays. This issue, only

seen with long chains of USB hubs, was never seen in practice. Timings in the host state machine have been

corrected to match USB spec. This fix is enabled by LINESTATE_TUNING.MULTI_HUB_FIX.

12.7.2.2.3. Device

• Added wake from suspend fix: Any bus activity (defined as K or SE0) should cause a wake from suspend, not just a

qualified period of resume signalling. This fix is enabled by default and can be disabled with

LINESTATE_TUNING.DEV_LS_WAKE_FIX (LS means line state in this instance, not low speed).
• Added DPSRAM double read feature to ensure data consistency. This avoids the need to set the AVAILABLE bit in the

buffer control register separate to the rest of the buffer information. This feature is enabled by default and

controlled by LINESTATE_TUNING.DEV_BUFF_CONTROL_DOUBLE_READ_FIX.

12.7. USB
1143

---
source_pdf: rp2350-datasheet-8.pdf
repository: llm_database
chapter: Chapter 12. Peripherals
section: 12.7.2. Changes from RP2040
pages: 1143-1145
type: technical_spec
generated_at: 2026-03-01T08:45:43.944040+00:00
---

# 12.7.2. Changes from RP2040

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

RP2350 Datasheet

• Added ability to stop DEVICE OUT FROM HOST when a short packet is received. For EP0 this is controlled by

SIE_CTRL.EP0_STOP_ON_SHORT_PACKET. This is done by stopping the transaction and then not toggling the

buffer if in double buffered mode. Also added short_packet interrupt to notify software that a short packet has been

received (INTS.RX_SHORT_PACKET)

12.7.2.2.4. Device error handling

• Added DEV_RX_ERR_QUIESCE feature: the device endpoint error count replicates the host’s internal Cerr count so

software can detect if the host has probably halted the endpoint after three consecutive errors. The various stages

of RX decode generate their own error signals that propagate to the top level. These error signals arrive at different

times, so two error interrupts generate for every failed transfer. Added an optional override for this behaviour by

forcing the device RX controller to idle after the first instance of an error during a transfer. This fix is enabled with

LINESTATE_TUNING.DEV_RX_ERR_QUIESCE.
• Added SIE_RX_CHATTER_SE0_FIX: the existing error recovery implementation waits for 8 FS idle bit-times before

signalling a framing error and returning to idle. This works OK for random bus errors, but when a hub terminates a

downstream packet, the hub forces a bit-stuff error followed by EOP. A valid token from the host may immediately

follow this, but the device controller may ignore it due to the enforced delay. Optionally waits for either a valid EOP

or 8 idle bit times before signalling a framing error. To enable the fix, use

LINESTATE_TUNING.SIE_RX_CHATTER_SE0_FIX.
• Fix RP2040-E15: the receive state machine doesn’t always handle cases where the bitstream deserialiser can abort

a transfer. If decoding terminates due to bitstuff errors during the middle phases of a packet, the device controller

can lock up. Unconditionally disables RX if the deserialiser has flagged a bitstuff error and subsequently signalled

framing error after linestate returns to idle. To enable this fix, use LINESTATE_TUNING.SIE_RX_BITSTUFF_FIX.
• Device state machine watchdog: added a watchdog so that if the device state machine gets stuck for a certain

amount of time it can be forced to idle. This is to handle any other error cases not anticipated by the above fixes.

To enable the watchdog, use DEV_SM_WATCHDOG.

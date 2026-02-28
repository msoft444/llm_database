---
source_pdf: rp2350-datasheet-1.pdf
repository: llm_database
chapter: Appendix C: Hardware revision history
section: Hardware Changes
pages: 1356-1356
type: technical_spec
generated_at: 2026-02-28T17:22:56.644477+00:00
---

# Hardware Changes

• Update the reset state of the following clock configuration registers:
◦ROSC: FREQA.DS0_RANDOM and FREQA.DS1_RANDOM from 0 to 1, enabling randomisation of first two drive
stages.
◦CLOCKS: CLK_SYS_CTRL.SRC from 0 to 1 (select AUX source).
◦CLOCKS: CLK_SYS_CTRL.AUXSRC from 0 to 2 (select ROSC as AUX source).
Bootrom changes
The A3 bootrom introduces the following changes:
• Fix RP2350-E10: UF2 drag-and-drop doesn’t work with partition tables. This previously required a workaround in
picotool, but the A3 bootrom no longer requires this workaround. picotool retains the workaround for compatibility
with A2.
• Fix RP2350-E13: a binary containing an explicitly invalid IMAGE_DEF followed by a valid IMAGE_DEF (in that order) fails
to boot.
• Fix RP2350-E14: the bootrom connect_internal_flash() function always uses pin 0, ignoring any configured
FLASH_DEVINFO CS1 chip select pin.
• Fix RP2350-E15: the bootrom otp_access() function applies incorrect access permission to pages 62 and 63.
• Fix RP2350-E19: RP2350 reboot halts if certain bits are set in FRCE_OFF when rebooting.
• Mitigate RP2350-E20: an attacker with physical access to the chip and the ability to physically glitch the CPU at
precise times could cause unsigned code execution on a secured RP2350 by targeting legitimate Non-secure calls
to the bootrom reboot() function.
• Mitigate RP2350-E21: an attacker with physical access to the chip and the ability to physically glitch the CPU at
precise times, could extract sensitive data from OTP on a RP2350 in BOOTSEL mode.
• Fix RP2350-E22: parsing a malformed lollipop block loop causes the system to halt rather than fail.
• Fix RP2350-E23: PICOBOOT GET_INFO command always returns zero for PACKAGE_SEL)
• Mitigate RP2350-E26: RCP random delays can create a side-channel. These delays are disabled in the bootrom.
• Update the early boot path to change the clk_ref divider from 1 to 5, and the ROSC divider from 8 to 2.
◦Together with the register reset state changes, this increases the boot clk_sys frequency by a factor of 4, to
approximately 48 MHz.
◦These changes reduce boot time and fault injection susceptibility.
◦These changes apply for all boot outcomes, including watchdog and POWMAN vector boot.
RP2350 A4
Stepping A4 is identified by a CHIP_ID.REVISION value of 0x8.
Hardware Changes
This stepping has no hardware changes.
RP2350 Datasheet
RP2350 A4
1355


---
source_pdf: rp2350-datasheet-1.pdf
repository: llm_database
chapter: Chapter 10. Security
section: 10.11. Decommissioning
pages: 875-876
type: technical_spec
generated_at: 2026-02-28T17:22:56.644477+00:00
---

# 10.11. Decommissioning

Table 977.
TRIG_FORCE Register
Bits
Description
Type
Reset
31:4
Reserved.
-
-
3:0
Simulate the firing of one or more detectors. Writing ones to this register will
set the matching bits in STATUS_TRIG.
If the glitch detectors are currently armed, writing ones will also immediately
reset the switched core power domain, and set the reset reason latches in
POWMAN_CHIP_RESET to indicate a glitch detector resets.
This register is Secure read/write only.
SC
0x0
10.10. Factory test JTAG
RP2350 contains JTAG hardware that is used to test devices after manufacturing. It is not a public interface, but its
capabilities are documented here for user risk assessment.
Much like the user-facing SWD debug, the JTAG interface is disabled at power-on, and enabled only once the OTP
power-on state machine has completed. If the CRIT1.SECURE_BOOT_ENABLE, CRIT1.SECURE_DEBUG_DISABLE or
CRIT1.DEBUG_DISABLE flag is set, then the JTAG interface remains held in reset indefinitely, so it cannot be
communicated with and cannot control internal hardware. The only way to re-enable the JTAG interface after setting
one of these critical flags is to set the RMA OTP flag (Section 10.11), which also permanently disables read and write
access to user OTP pages. The RMA flag itself is write-protected using the page 63 protection flags, so you can prevent
untrusted software from programming the RMA flag.
To take the JTAG interface out of reset, write to bit 0 of the RP-AP control register, accessed via SWD. To connect the
JTAG interface to GPIOs (TCK, TMS, TDI, TDO on GPIO0 → 3), set bit 1 of the RP-AP control register. The RP-AP is always
accessible, even when external debug is disabled, because it is also used to enter the debug keys (Section 3.5.9.2).
However, attempts to remove the JTAG reset are ignored when any of the aforementioned critical OTP flags are set.
The JTAG interface provides:
• Standard test capabilities such as IDCODE and EXTEST (boundary scan); these are not guaranteed to be IEEE-
compliant, as this is an internal factory test interface, not a user-facing debug port
• Full AHB bus access, with Secure and Privileged attributes and an HMASTER of 3 (debugger)
• Asynchronous access to a small subset of register controls, generally limited to clocks, oscillators and reset
controls
The JTAG interface’s AHB bus access is muxed in place of the DMA read port, when the JTAG interface is enabled.
Any and all details of the factory test JTAG interface, with the exception of which OTP flags disable and re-enable it, are
subject to change with revisions of the RP2350 silicon.
10.11. Decommissioning
Devices returned to Raspberry Pi Ltd for fault analysis must be decommissioned before return, to restore factory test
functionality. A device is decommissioned by programming the OTP PAGE63_LOCK0.RMA flag to 1. Return may be
requested by Raspberry Pi Ltd when diagnosing systematic issues across a population of devices.
Setting the RMA flag has two effects:
• The factory test JTAG interface is re-enabled, irrespective of the values of any CRIT1 flags.
• Pages 3 through 61 become permanently inaccessible: this is all pages that do not have predefined contents listed
in Section 13.10.
RP2350 Datasheet
10.10. Factory test JTAG
874


The effect on OTP contents is as though all had been promoted to the inaccessible lock level:
• write attempts will fail
• read attempts will return all-ones, when read via an unguarded read alias, or bus faults, when read via a guarded
read alias
The logic that disables OTP access and the logic that re-enables the test interface are driven from the same signal
internally, so this bit does not provide external access to user OTP contents, provided no sensitive material is stored in
pages 0, 1, 2, 62 or 63. Setting the RMA flag is irreversible, and may render the device permanently unusable, if it is
configured to boot from OTP contents stored in pages 3 through 61.
After setting the RMA flag, test the OTP access (e.g. via the SWD interface) and verify for yourself that any sensitive
data stored in OTP has been made inaccessible.
The page 63 lock word has no other function besides RMA because pages 62/63 contain the lock words themselves,
each of which is protected by its own permissions. This means the RMA flag can be write-protected by setting either a
hard or soft lock on page 63.
RP2350 Datasheet
10.11. Decommissioning
875


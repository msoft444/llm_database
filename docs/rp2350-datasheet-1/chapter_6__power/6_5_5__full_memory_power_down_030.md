---
source_pdf: rp2350-datasheet-1.pdf
repository: llm_database
chapter: Chapter 6. Power
section: 6.5.5. Full memory power down
pages: 491-491
type: technical_spec
generated_at: 2026-02-28T17:22:56.644477+00:00
---

# 6.5.5. Full memory power down

If you halt the crystal oscillator (XOSC), you must also halt the PLLs to prevent them losing lock when their input
reference clock stops. The PLL VCO may behave erratically when the frequency reference is lost, such as increasing to
a very high frequency. Reconfigure and re-enable the PLLs after the XOSC starts again. Do not attempt to run clocks
from the PLLs while the XOSC is stopped.
The DORMANT state is entered by writing a keyword to the DORMANT register in whichever oscillator is active: ring
oscillator (Section 8.3) or crystal oscillator (Section 8.2). If both are active, the one providing the processor clock must
be stopped last because it will stop software from executing.
6.5.3.1. Waking from the DORMANT state
The system exits the DORMANT state on any of the following events:
• an alarm from the AON Timer which causes TIMER.ALARM to assert
• the assertion of an interrupt from GPIO Bank 0 to the DORMANT_WAKE interrupt destination
• the assertion of an interrupt from GPIO Bank 1 to the DORMANT_WAKE interrupt destination
When waking from the AON Timer you do not have to enable the IRQ output from POWMAN. It is sufficient for the timer
to fire, without being mapped to an interrupt output. Any AON Timer alarm comparison event which causes
TIMER.ALARM to assert causes the system to exit the DORMANT state. It is the actual alarm event which causes the
exit, not the TIMER.ALARM status; if you enter the DORMANT state with the TIMER.ALARM status set to 1, but the timer
alarm comparison logic disabled by TIMER.ALARM_ENAB, you will not exit the DORMANT state.
The GPIO Bank registers have interrupt enable registers for interrupts targeting the DORMANT mode wake logic, such
as DORMANT_WAKE_INTE0. These are identical to the interrupt enable registers for interrupts targeting the processors,
such as PROC0_INTE0.
Waking from the DORMANT state restarts the oscillator which was disabled by entry to the DORMANT state. It does not
restart any other oscillators, or change any system-level clock configuration.
6.5.4. Memory periphery power down
The main system memories (SRAM0 → SRAM9, mapped to bus addresses 0x20000000 to 0x20081fff), as well as the USB
DPRAM, can be partially powered down via the MEMPOWERDOWN register in the SYSCFG registers (see Section
12.15.2). This powers down the analogue circuitry used to access the SRAM storage array (the periphery of the SRAM)
but the storage array itself remains powered. Memories retain their current contents, but cannot be accessed. Static
power is reduced.
CAUTION
Memories must not be accessed when powered down. Doing so can corrupt memory contents.
When powering a memory back up, a 20ns delay is required before accessing the memory again.
The XIP cache (see Section 4.4) can also be powered down, with CTRL.POWER_DOWN. The XIP hardware will not
generate cache accesses whilst the cache is powered down. Note that this is unlikely to produce a net power savings if
code continues to execute from XIP, due to the comparatively high voltages and switching capacitances of the external
QSPI bus.
6.5.5. Full memory power down
RP2350 can completely power down its internal SRAM. Unlike the memory periphery power down described in Section
6.5.4, this completely disconnects the SRAM from the power supply, reducing static power to near zero.
Contents are lost when fully powering down memories. When you power memories up again following a power down,
RP2350 Datasheet
6.5. Power reduction strategies
490


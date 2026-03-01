---
source_pdf: rp2350-datasheet-10.pdf
repository: llm_database
chapter: Chapter 7. Resets
section: 7.3.3. Chip-level reset sources
pages: 497-498
type: technical_spec
generated_at: 2026-03-01T12:20:14.494106+00:00
---

# 7.3.3. Chip-level reset sources

7.3.3. Chip-level reset sources

In order of severity, the following events can trigger a chip-level reset:

Power-On Reset (POR)

The power-on reset ensures the chip starts up cleanly when power is first applied by holding it in reset until the

digital core supply (DVDD) reaches a voltage high enough to reliably power the chip’s core logic. The POR

component is described in detail in Section 7.6.1, “Power-on reset (POR)”.

7.3. Chip-level resets
496

RP2350 Datasheet

Brownout Detection (BOD)

The brownout detector prevents unreliable operation when the digital core supply (DVDD) drops below a safe

operating level. The BOD component is described in detail in Section 7.6.2, “Brownout detection (BOD)”. The reset

asserted by the BOD is referred to as the brownout reset, or BOR.

External Reset

The chip can be reset by taking the RUN pin low. This holds the chip in reset irrespective of the state of the core

power supply (DVDD), the power-on reset block, and brownout detection block. RUN can be used to extend the initial

power-on reset, or can be driven from an external source to start and stop the chip as required. If RUN is not used, it

should be tied high. Double-tapping the RUN low will set CHIP_RESET.DOUBLE_TAP. Boot code reads this flag and

selects an alternate boot sequence if the flag is set.

Debugger Reset Request

The debugger is able to initiate a chip-level reset using the CDBGPWRUPREQ control. For more information, see Section

3.5, “Debug”.

Rescue Debug Port Reset

The chip can also be reset via the Rescue Debug Port. This allows the chip to be recovered from a locked-up state.

In addition to resetting the chip, a Rescue Debug Port reset also sets CHIP_RESET.RESCUE_FLAG. This is checked

by boot code at startup, causing it to enter a safe state if the bit is set. See Section 3.5.8, “Rescue reset” for more

information.

Watchdog

The watchdog can trigger various levels of chip-level reset by setting appropriate bits in the WDSEL register. A chip-

level reset triggered by a watchdog reset will reset the watchdog and the watchdog scratch registers. Additional

general purpose scratch registers are available in POWMAN. These are not reset by a chip-level reset triggered by the

watchdog.

SWCORE Powerdown

For a list of operations that power down the switched-core power domain (SWCORE) and trigger this reset, see

Section 6.2, “Power management”.

Glitch Detector

This reset fires if a glitch is detected in SWCORE power supply. For more information, see Section 10.9, “Glitch

detector”.

RISC-V Non-Debug-Module Reset

The dmcontrol.ndmreset bit in the RISC-V Debug Module resets all RISC-V harts in the system. It resets no other

hardware. However, it is recorded as a chip-level reset reason in CHIP_RESET.HAD_HZD_SYS_RESET_REQ. See

Section 3.5.3, “RISC-V debug” for details of the RISC-V debug subsystem.

The source of the last chip-level reset is recorded in the CHIP_RESET register.

A complete list of POWMAN registers is provided in Section 6.4, “Power management (POWMAN) registers”.

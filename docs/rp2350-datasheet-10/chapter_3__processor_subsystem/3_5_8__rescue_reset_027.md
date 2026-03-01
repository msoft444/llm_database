---
source_pdf: rp2350-datasheet-10.pdf
repository: llm_database
chapter: Chapter 3. Processor subsystem
section: 3.5.8. Rescue reset
pages: 91-91
type: technical_spec
generated_at: 2026-03-01T12:20:14.494106+00:00
---

# 3.5.8. Rescue reset

3.5.8. Rescue reset

A rescue reset is a full system reset, similar to asserting the RUN pin low, which also sets a flag telling the bootrom to

halt before running any user software. This is performed over the SWD bus using the RP-AP, and can be performed even

when system clocks are stopped and the switched core power domain is powered down. This is used in the case where

the chip has locked up, for example if code has been programmed into flash which permanently halts the system clock:

since the debugger can no longer communicate with the processors to return the system to a working state, more

drastic action is needed. This functionality was provided by the Rescue DP on RP2040, but on RP2350 it is provided by

the RP-AP, to avoid mandatory use of multidrop SWD.

A rescue is invoked by setting and then clearing the CTRL.RESCUE_RESTART bit in the RP-AP. This causes a hard reset

of the chip, and sets CHIP_RESET.RESCUE_FLAG to indicate that a rescue reset took place. The bootrom checks this

flag almost immediately in the initial boot process (before watchdog, flash or USB boot), acknowledges by clearing the

bit, then halts the processor. This leaves the system in a safe state, with the system clock running, so that the debugger

can reattach to the cores and load fresh code.

3.5. Debug
90

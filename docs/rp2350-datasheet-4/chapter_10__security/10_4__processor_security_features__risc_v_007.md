---
source_pdf: rp2350-datasheet-4.pdf
repository: llm_database
chapter: Chapter 10. Security
section: 10.4. Processor security features (RISC-V)
pages: 822-822
type: technical_spec
generated_at: 2026-03-01T03:27:49.792955+00:00
---

# 10.4. Processor security features (RISC-V)

10.4. Processor security features (RISC-V)

The Hazard3 processors on RP2350 implement the following standard RISC-V security features:

• Machine and User execution modes (M-mode and U-mode)
• The Physical Memory Protection unit (PMP)

M-mode has full access to the processor’s internal status registers, but U-mode does not. The processor’s bus

accesses are tagged with its current execution mode and filtered by ACCESSCTRL bus filters, as described in Section

10.6.2.

The processor starts in M-mode, and enters M-mode upon taking any trap (exception or interrupt). It enters U-mode only

by executing a return-from-M-mode instruction, mret, with previous privilege set to U-mode. This means all interrupts

initially target M-mode, but can be de-privileged to U-mode via software routing. Because stacks are software-managed

on RISC-V, software cooperation is required to fully separate the two execution contexts, though there are enough

hardware hooks to make this possible. For more details about interrupts and exceptions on RISC-V, and how they relate

to the core’s privilege levels, see Section 3.8.4.

The PMP is a memory protection unit built into each RISC-V processor that filters every instruction execution address

and every load/store address against a list of permission regions. The Hazard3 instances on RP2350 are configured

with 8 PMP regions each, with a 32-byte granule and naturally-aligned power-of-2 region support only.

Additionally, there are 3 PMP-hardwired regions, which set a default User-mode RW permission on peripherals and a

10.3. Overview (RISC-V)
821

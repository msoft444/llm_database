---
source_pdf: rp2350-datasheet-1.pdf
repository: llm_database
chapter: Chapter 5. Bootrom
section: 5.2.4. Watchdog boot vector
pages: 372-372
type: technical_spec
generated_at: 2026-02-28T17:22:56.644477+00:00
---

# 5.2.4. Watchdog boot vector

5.2.2.2. Differences between Arm and RISC-V
The boot sequence outlined in Table 451 has the following differences on RISC-V:
• Secure boot is not supported (from any image source).
• Anti-rollback checking is not supported as it applies only to secure boot.
• Additional security checks such as the use of the RCP to validate booleans are disabled.
• The UART and USB bootloaders continue to run in Machine mode, rather than transitioning from the Arm Secure to
Non-secure state, meaning there is no hardware-enforced security boundary between these boot phases.
• The XIP setup function written to boot RAM on a successful flash boot contains RISC-V rather than Arm
instructions.
5.2.3. POWMAN boot vector
POWMAN contains scratch registers similar to the watchdog scratch registers, which persist over power-down of the
switched core power domain, in addition to most system resets. These registers allow users to install their own boot
handler, and divert control away from the main boot sequence on non-POR/BOR resets. It recognises the following
values written to BOOT0 through BOOT3:
• BOOT0: magic number 0xb007c0d3
• BOOT1: Entry point XORed with magic -0xb007c0d3 (0x4ff83f2d)
• BOOT2: Stack pointer
• BOOT3: Entry point
Use this to vector into code preloaded in RAM which was retained during a low-power state.
If either of the magic numbers mismatch, POWMAN vector boot does not take place. If the numbers match, the
bootrom zeroes BOOT0 before entering the vector, so that the behaviour does not persist over subsequent reboots.
The POWMAN boot vector is permitted to return. The boot sequence continues as normal after a return from POWMAN
vector boot, as though the vector boot had not taken place. There is no requirement for the vector to preserve the global
pointer (gp) register on RISC-V. Use this to perform any additional setup required for the boot path, such as issuing a
power-up command to an external QSPI device that may have been powered down (for example, through a B9h power-
down command).
The entry point (pc) must have the LSB set on Arm (the Thumb bit) and clear on RISC-V. If this condition is not met, the
bootrom assumes you have passed a RISC-V function pointer to an Arm processor (or vice versa) and hangs the core
rather than continuing, since executing code for the wrong architecture has spectacularly undefined consequences.
The linker should automatically set the Thumb bit appropriately for a function pointer relocation, but this is something to
be aware of if you pass hardcoded values such as the base of SRAM: this is correctly passed as 0x20000001 on Arm
(Thumb bit set) and 0x20000000 on RISC-V (no Thumb bit, halfword-aligned).
5.2.4. Watchdog boot vector
Watchdog boot allows users to install their own boot handler, and divert control away from the main boot sequence on
non-POR/BOR resets. It recognises the following values written to the watchdog’s upper scratch registers:
• SCRATCH4: magic number 0xb007c0d3
• SCRATCH5: entry point XORed with magic -0xb007c0d3 (0x4ff83f2d)
• SCRATCH6: stack pointer
• SCRATCH7: entry point
If either of the magic numbers mismatch, watchdog boot does not take place. If the numbers match, the Bootrom
RP2350 Datasheet
5.2. Processor-controlled boot sequence
371


---
source_pdf: rp2350-datasheet-10.pdf
repository: llm_database
chapter: Chapter 3. Processor subsystem
section: 3.9. Arm/RISC-V architecture switching
pages: 336-336
type: technical_spec
generated_at: 2026-03-01T12:20:14.494106+00:00
---

# 3.9. Arm/RISC-V architecture switching

3.9. Arm/RISC-V architecture switching

RP2350 supports both Arm and RISC-V processor architectures. SDK-based programs that don’t contain assembly code

typically run unmodified on either architecture by providing the appropriate build flag.

There are two processor sockets on RP2350, referred to as core 0 and core 1 throughout this document. Each socket

can be occupied either by a Cortex-M33 processor (implementing the Armv8-M Main architecture, plus extensions) or by

a Hazard3 processor (implementing the RV32IMAC architecture, plus extensions).

When a processor reset is removed, hardware samples the ARCHSEL register in the OTP control register block to

determine which processor to connect to that socket. The unused processor is held in reset indefinitely, with its clock

inputs gated. The default and allowable values of the ARCHSEL register are determined by critical OTP flags:

1. If CRIT0_ARM_DISABLE is set, only RISC-V is allowed.

2. Else if CRIT0_RISCV_DISABLE is set, only Arm is allowed.

3. Else if CRIT1_SECURE_BOOT_ENABLE is set, only Arm is allowed.

4. Else if CRIT1_BOOT_ARCH is set, both architectures are permitted, and the default is RISC-V.

5. If none of the above flags are set, both architectures are permitted, and the default is Arm.

No CRIT1 flags are set by default, so on devices where both architectures are available, the default is Arm. To change the

default architecture to RISC-V, set the CRIT1_BOOT_ARCH flag to 1.

Enabling secure boot disables the RISC-V cores because the RP2350 bootrom does not implement secure boot for

RISC-V. This prevents a bad actor from side-stepping secure boot by switching architectures.

NOTE

As of RP2350 A3 the CRIT0_ARM_DISABLE flag has no effect, removing a potential unlock path for debug on a secured

RP2350. Additionally, the combination of CRIT0_RISCV_DISABLE=1 and CRIT1_BOOT_ARCH=1 is decoded to an invalid state,

preventing boot.

RP2350 only samples the ARCHSEL register when a processor is reset. Its value is ignored at all other times, so

software can program the register before a watchdog reset to implement a software-initiated switch between

architectures.

Read the ARCHSEL_STATUS register to check the ARCHSEL value most recently sampled by each processor.

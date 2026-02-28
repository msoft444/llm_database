---
source_pdf: rp2350-datasheet-2.pdf
repository: llm_database
chapter: Chapter 3. Processor subsystem
section: 3.9.1. Automatic switching
pages: 336-336
type: technical_spec
generated_at: 2026-02-28T17:43:10.557178+00:00
---

# 3.9.1. Automatic switching

![Page 336 figure](images/fig_p0336.png)

RP2350 Datasheet

| Bits | Description | Type | Reset |
| --- | --- | --- | --- |
| 31:0 | Pointer to configuration data structure (hardwired to 0) | RO | 0x00000000 |

Table 433.

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

3.9.1. Automatic switching

RP2350 binaries contain a binary marker recognised by the bootrom. This marker:

• contains additional information such as the binary’s entry point and the intended architecture: Arm, RISC-V, or both
• helps detect when a flash device is connected
• helps verify that the flash device was accessed using the correct SPI parameters

When booting with core 0 in Arm architecture mode, upon detecting a bootable RISC-V binary, the bootrom

automatically resets both cores and switches them to RISC-V architecture mode. After the reset, the bootrom detects

that the binary and processor architectures match, so the binary launches normally.

Likewise, when booting with core 0 in RISC-V architecture mode, upon detecting a bootable Arm binary, the bootrom

automatically resets both cores and switches them to Arm architecture mode.

3.9. Arm/RISC-V architecture switching
335

---
source_pdf: rp2350-datasheet-1.pdf
repository: llm_database
chapter: Chapter 4. Memory
section: 4.3.1. List of registers
pages: 340-340
type: technical_spec
generated_at: 2026-02-28T17:22:56.644477+00:00
---

# 4.3.1. List of registers

NOTE
Memory in the peripheral address space (addresses starting with 0x4, 0x5 or 0xd) does not support code execution.
This includes USB RAM and boot RAM. These address ranges are made IDAU-Exempt to simplify assigning
peripherals to security domains using ACCESSCTRL, and consequently must be made non-executable to avoid the
possibility of Non-secure-writable, Secure-executable memory.
4.3. Boot RAM
Boot RAM is a 1 kB (256 × 32-bit) SRAM dedicated for use by the bootrom. It is slower than main SRAM, as it is
accessed over APB, taking three cycles for a read and four cycles for a write.
Boot RAM is used for myriad purposes during boot, including the initial pre-boot stack. After the bootrom enters the
user application, boot RAM contains state for the user-facing ROM APIs, such as the resident partition table used for
flash programming protection, and a copy of the flash XIP setup function (formerly known as boot2) to quickly re-
initialise flash XIP modes following serial programming operations.
Boot RAM is hardwired to permit Secure access only (Arm) or Machine-mode access only (RISC-V). It is physically
impossible to execute code from boot RAM, regardless of MPU configuration, as it is on the APB peripheral bus
segment, which is not wired to the processor instruction fetch ports.
Since boot RAM is in the XIP RAM power domain, it is always powered when the switched core domain is powered. This
simplifies SRAM power management in the bootrom, because it doesn’t have to power up any RAM before it has a place
to store the call stack.
Boot RAM supports the standard atomic set/clear/XOR accesses used by other peripherals on RP2350 (Section 2.1.3).
It is possible to use boot RAM for user-defined purposes, but this is not recommended, as it may cause ROM APIs to
behave unpredictably. Calling into the ROM could modify data stored in boot RAM.
4.3.1. List of registers
A small number of registers are located on the same bus endpoint as boot RAM:
Write Once Bits
These are flags which once set, can only be cleared by a system reset. They are used in the implementation of
certain bootrom security features.
Boot Locks
These function the same as the SIO spinlocks (Section 3.1.4), however they are normally reserved for bootrom
purposes (Section 5.4.4).
These registers start from an offset of 0x800 above the boot RAM base address of 0x400e0000 (defined as
BOOTRAM_BASE in the SDK).
Table 435. List of
BOOTRAM registers
Offset
Name
Info
0x800
WRITE_ONCE0
This registers always ORs writes into its current contents. Once a
bit is set, it can only be cleared by a reset.
0x804
WRITE_ONCE1
This registers always ORs writes into its current contents. Once a
bit is set, it can only be cleared by a reset.
0x808
BOOTLOCK_STAT
Bootlock status register. 1=unclaimed, 0=claimed. These locks
function identically to the SIO spinlocks, but are reserved for
bootrom use.
RP2350 Datasheet
4.3. Boot RAM
339


---
source_pdf: rp2350-datasheet-3.pdf
repository: llm_database
chapter: Chapter 3. Processor subsystem
section: 3.5.3. RISC-V debug
pages: 87-87
type: technical_spec
generated_at: 2026-03-01T02:30:35.833805+00:00
---

# 3.5.3. RISC-V debug

RP2350 Datasheet

2. The 128-bit Selection Alert sequence: 0x19bc0ea2, 0xe3ddafe9, 0x86852d95, 0x6209f392, LSB-first.

3. Four SWCLK cycles with SWDIO low.

4. SWD activation code sequence : 0x1a, LSB first.

5. At least 50 × SWCLK cycles with SWDIO high (line reset).

6. A DPIDR read to exit the Reset state

In order to wake up the system from a low power (P1.x) state, set the CDBGPWRUPREQ in the DP CTRL/STAT register,

then poll CDBGPWRUPACK in the same register until set. In low-power states, only the SW-DP and RP-AP are accessible,

as the remaining debug logic is unpowered.

3.5.2. Arm debug

There are two AHB5 Mem-APs, at offsets 0x02000 and 0x04000 in the debug address space, which are used to debug the

two Arm Cortex-M33 processors. Each Mem-AP is an AHB5 manager which accesses a 32-bit downstream address

space. This is the same address space accessed by a processor’s load/store instructions, which includes system-level

hardware such as memory and peripherals, and processor-internal hardware on the processor’s private peripheral bus

(PPB). Certain PPB registers are visible only when accessed from the Mem-AP, not when accessed by software running

on the processor.

The AHB5 Mem-AP’s own register map is defined in Arm’s ADIv6 specification. Generally this is only of interest to those

implementing their own debug translator, and the Mem-AP can be thought of simply as a bridge between a DP (such as

RP2350’s SW-DP) and a downstream address space.

The standard Arm debug registers used to debug software running on the Cortex-M33 can be found documented in the

Armv8-M Architecture Reference Manual, or the Cortex-M33 Technical Reference Manual, available from Arm Ltd. This

datasheet also documents the core’s internal registers in Section 3.7.5.

The Mem-APs can access system peripherals and memory at exactly the same addresses they would be accessed by

software running on the processor. However, the privilege and security of Mem-AP accesses may be different from the

security state of the software running on the processor at the point it halted: the privilege and security of Mem-AP

accesses is configured explicitly via its control and status word (CSW) register. Care must be taken when debugging

Non-secure software which accesses the SIO, for example, because by default the debugger may access the Secure

alias of the SIO, not the Non-secure alias which software will have been accessing.

The bus filters configured by the ACCESSCTRL bus access permission registers (Section 10.6.2) treat bus accesses

originating from the Mem-APs as distinct from bus accesses originating from software running on the processor. This

means it is possible to lock software out from a peripheral, whilst still allowing debugger access.

3.5.3. RISC-V debug

There is a single APB Mem-AP, at offset 0x0a000 in the debug address space, which provides access only to the RISC-V

Debug Module (DM). The DM is a standard component which the debugger uses to enumerate RISC-V harts present in

the system, debug software running on each hart, and access the system bus. It is defined in the RISC-V debug

specification, of which RP2350 implements version 0.13.2.

From the point of view of the RISC-V debug specification, the SW-DP and APB Mem-AP function jointly as the Debug

Transport Module for this system. The DM is located at offset 0x0 in the APB-AP’s downstream address space, and the

registers are word-sized and byte-addressed, meaning the DM register addresses in the debug specification must be

multiplied by 4 to get the correct APB address.

On RP2350, each core possesses exactly one hardware thread (hart). Core 0 has a hart ID of 0, and core 1 has a hart ID

of 1. These hart IDs match the hart index used in the DM. This DM is also equipped with the hart array mask select

extension, which allows multiple cores to be reset/halted/resumed simultaneously.

The DM is equipped with the System Bus Access (SBA) extension, which allows the debugger to access the system bus

without halting either core. This can be used for minimally intrusive debug techniques like Segger RTT. SBA accesses

3.5. Debug
86

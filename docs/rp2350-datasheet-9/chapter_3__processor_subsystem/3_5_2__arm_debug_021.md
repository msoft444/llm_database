---
source_pdf: rp2350-datasheet-9.pdf
repository: llm_database
chapter: Chapter 3. Processor subsystem
section: 3.5.2. Arm debug
pages: 87-87
type: technical_spec
generated_at: 2026-03-01T11:44:31.548839+00:00
---

# 3.5.2. Arm debug

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

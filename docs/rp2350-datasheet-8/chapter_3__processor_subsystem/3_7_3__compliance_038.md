---
source_pdf: rp2350-datasheet-8.pdf
repository: llm_database
chapter: Chapter 3. Processor subsystem
section: 3.7.3. Compliance
pages: 129-132
type: technical_spec
generated_at: 2026-03-01T08:45:43.944040+00:00
---

# 3.7.3. Compliance

3.7.3. Compliance

The processor complies with, or implements, the relevant Arm architectural standards and protocols, and relevant

external standards.

3.7. Cortex-M33 processor
128

RP2350 Datasheet

3.7.3.1. Arm architecture

The processor is compliant with the following:

• Armv8-M Main Extension
• Armv8-M Security Extension
• Armv8-M Protected Memory System Architecture (PMSA)
• Armv8-M Floating-point Extension
• Armv8-M Digital Signal Processing (DSP) Extension
• Armv8-M Debug Extension
• Armv8-M Flash Patch Breakpoint (FPB) architecture version 2.0

3.7.3.2. Bus architecture

The processor provides external interfaces that comply with the AMBA 5 AHB5 protocol. The processor also

implements interfaces for CoreSight and other debug components using the APB4 protocol and ATBv1.1 part of the

AMBA 4 ATB protocol.

For more information, see the:

• Arm AMBA 5 AHB Protocol Specification
• AMBA APB Protocol Version 2.0 Specification
• Arm AMBA 4 ATB Protocol Specification ATBv1.0 and ATBv1.1

The processor also provides a Q-Channel interface. For more information, see the AMBA Low Power Interface

Specification.

3.7.3.3. Debug

The debug features of the processor implement the Arm Debug Interface Architecture. For more information, see the

Arm Debug Interface Architecture Specification, ADIv5.0 to ADIv5.2.

3.7.3.4. Embedded Trace Macrocell

The trace features of the processor implement the Arm Embedded Trace Macrocell (ETM) v4.2 architecture.

For more information, see the Arm CoreSight ETM-M33 Technical Reference Manual.

3.7.3.5. Floating-point unit

The Cortex-M33 processor with FPU supports single-precision arithmetic as defined by the FPv5 architecture that is part

of the Armv8-M architecture. The FPU provides floating-point computation functionality compliant with ANSI/IEEE

Standard 754-2008, IEEE Standard for Binary Floating-Point Arithmetic.

The FPU supports single-precision add, subtract, multiply, divide, multiply and accumulate, and square root operations.

It also provides conversions between fixed-point and floating-point data formats, and floating-point constant

instructions.

The FPU provides an extension register file containing 32 single-precision registers.

The registers can be viewed as:

3.7. Cortex-M33 processor
129

RP2350 Datasheet

• Thirty-two 32-bit single-word registers, S0-S31
• Sixteen 64-bit double-word registers, D0-D15
• A combination of registers from these views

3.7.3.5.1. FPU modes

The FPU provides full-compliance, flush-to-zero, and Default NaN modes of operation. In full-compliance mode, the FPU

processes all operations according to the IEEE 754 standard in hardware.

Modes of operation are controlled using the Floating-Point Status and Control Register, FPSCR.

Setting the FPSCR.FZ bit enables Flush-to-Zero (FZ) mode. In FZ mode, the FPU treats all subnormal input operands of

arithmetic operations as zeros. Exceptions that result from a zero operand are signalled appropriately. VABS, VNEG, and

VMOV are not considered arithmetic operations and are not affected by FZ mode. When an operation yields a tiny result

(as described in the IEEE 754 standard, where the destination precision is smaller in magnitude than the minimum

normal value before rounding) FZ mode replaces the result with a zero.

The FPSCR.IDC bit indicates when an input flush occurs.

The FPSCR.UFC bit indicates when a result flush occurs.

Setting the FPSCR.DN bit enables Default NaN (DN) mode. In NaN mode, the result of any arithmetic data processing

operation that involves an input NaN, or that generates a NaN result, returns the default NaN. All arithmetic operations

except for VABS, VNEG, and VMOV ignore the fraction bits of an input NaN.

Setting neither the FPSCR.DN bit nor the FPSCR.FZ bit enables full-compliance mode. In full-compliance mode, FPv5

functionality is compliant with the IEEE 754 standard in hardware.

For more information about the FPU and FPSCR, see the Armv8-M Architecture Reference Manual.

3.7.3.5.2. FPU exceptions

The FPU sets the cumulative exception status flag in the FPSCR register as required for each instruction, in accordance

with the FPv5 architecture. The FPU does not support exception traps.

The processor has six output pins. By default, they are disconnected. Each reflect the status of one of the cumulative

exception flags:

FPIXC

Masked floating-point inexact exception.

FPUFC

Masked floating-point underflow exception.

FPOFC

Masked floating-point overflow exception.

FPDZC

Masked floating-point divide by zero exception.

FPIDC

Masked floating-point input denormal exception.

FPIOC

Invalid operation.

When a floating-point context is active, the stack frame extends to accommodate the floating-point registers. To reduce

the additional interrupt latency associated with writing the larger stack frame on exception entry, the processor

supports lazy stacking. This means that the processor reserves space on the stack for the FP state, but does not save

that state information to the stack unless the processor executes an FPU instruction inside the exception handler.

3.7. Cortex-M33 processor
130

RP2350 Datasheet

The lazy save of the FP state is interruptible by a higher priority exception. The FP state saving operation starts over

after that exception returns.

3.7.3.5.3. Low power FPU operation

If the FPU is in a separate power domain, the way the FPU domain powers down depends on whether the FPU domain

includes state retention logic.

To power down the FPU:

• If FPU domain includes state retention logic, disable the FPU by clearing the CPACR.CP10 and CPACR.CP11 bitfields.
• If FPU domain does not include state retention logic, disable the FPU by clearing the CPACR.CP10 and CPACR.CP11

bitfields and set both the CPPWR.SU10 and CPPWR.SU11 bitfields to 1.

WARNING

Setting the CPPWR.SU10 and CPPWR.SU11 bitfields indicates that FPU state can be lost.

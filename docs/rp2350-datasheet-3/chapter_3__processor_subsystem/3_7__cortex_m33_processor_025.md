---
source_pdf: rp2350-datasheet-3.pdf
repository: llm_database
chapter: Chapter 3. Processor subsystem
section: 3.7. Cortex-M33 processor
pages: 124-124
type: technical_spec
generated_at: 2026-03-01T02:30:35.833805+00:00
---

# 3.7. Cortex-M33 processor

![Page 124 figure](images/fig_p0124.png)

RP2350 Datasheet

mcr p7, #5, r0, CRn, CRm, #1

3.6.3.7.7. Panic

rcp_panic

Stalls the coprocessor port forever. If the processor abandons the coprocessor access, asserts NMI and continues

stalling the coprocessor port. Also immediately raises an RCP fault on other cores.

Opcode:

cdp p7, #0, c0, c0, c0, #1

Software executes an rcp_panic instruction when it detects a condition that makes it unsafe to continue executing

the current program. The RCP responds by stalling the processor’s CDP access forever, which should cause the

processor to stop fetching and executing instructions.

The processor is allowed to abandon a stalled coprocessor instruction when interrupted, which may cause it to

continue executing in an unsafe state. The RCP responds to an abandoned transfer by asserting the non-maskable

interrupt, pre-empting the interrupt handler that caused the coprocessor access to be abandoned. This should

swiftly encounter another RCP instruction and once again stall the processor, this time without allowing

interruption.

Panic is specified in this way, instead of gating the processor clock, so the debugger can still attach cleanly to the

processor after a panic.

3.6.4. Floating point unit

The Cortex-M33 cores on RP2350 are configured with the standard Arm single-precision floating point unit (FPU).

Coprocessor ports 10 and 11 access the FPU.

The Arm floating point extension is documented in the Armv8-M Architecture Reference Manual.

Applications built with the SDK use the FPU automatically by default. For example, calculations with the float data type

in C automatically use the standard FPU, while calculations with the double data type automatically use the RP2350

double-precision coprocessor (Section 3.6.2).

3.7. Cortex-M33 processor

Arm Documentation

Much of the following is excerpted from the Cortex-M33 Technical Reference Manual. Used with

permission.

The Arm Cortex-M33 processor is a low gate count, highly energy-efficient processor intended for microcontroller and

embedded applications. The processor is based on the Armv8-M architecture and is primarily for use in environments

where security is an important consideration.

3.7. Cortex-M33 processor
123

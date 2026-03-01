---
source_pdf: rp2350-datasheet-10.pdf
repository: llm_database
chapter: Chapter 3. Processor subsystem
section: 3.7.4. Programmer’s model
pages: 132-149
type: technical_spec
generated_at: 2026-03-01T12:20:14.494106+00:00
---

# 3.7.4. Programmer’s model

3.7.4. Programmer’s model

The Cortex-M33 programmer’s model is an implementation of the Armv8-M Main Extension architecture.

For a complete description of the programmers model, refer to the Armv8-M Architecture Reference Manual, which also

contains the Armv8-M Thumb instructions. In addition, other options of the programmers model are described in the

System Control, MPU, NVIC, FPU, Debug, DWT, ITM, and TPIU feature topics.

3.7.4.1. Modes of operation and execution

The Cortex-M33 processor supports Secure and Non-secure security states, Thread and Handler operating modes, and

can run in either Thumb or Debug operating states. In addition, the processor can limit or exclude access to some

resources by executing code in privileged or unprivileged mode.

See the Armv8-M Architecture Reference Manual for more information about the modes of operation and execution.

3.7.4.1.1. Security states

With the Armv8-M Security Extension, the programmer’s model includes two orthogonal security states: Secure state

and Non-secure state. The processor always resets into Secure state. Each security state includes a set of independent

operating modes and supports both privileged and unprivileged user access. Registers in the System Control Space are

banked across Secure and Non-secure state, with a Non-secure register view available to Secure state at an aliased

address.

3.7.4.1.2. Operating modes

For each security state, the processor can operate in Thread or Handler mode. The following conditions cause the

processor to enter Thread or Handler mode:

• The processor enters Thread mode on reset, or as a result of an exception return to Thread mode. Privileged and

Unprivileged code can run in Thread mode.
• The processor enters Handler mode as a result of an exception. In Handler mode, all code is privileged.

The processor can change security state on taking an exception, for example when a Secure exception is taken from

Non-secure state, the Thread mode enters the Secure state Handler mode. The processor can also call Secure functions

3.7. Cortex-M33 processor
131

RP2350 Datasheet

from Non-secure state and Non-secure functions from Secure state. The Security Extension includes requirements for

these calls to prevent Secure data from being accessed in Non-secure state.

3.7.4.1.3. Operating states

The processor can operate in Thumb or Debug state:

• Thumb state is the state of normal execution running 16-bit and 32-bit halfword- aligned Thumb instructions.
• Debug state is the state when the processor is in Halting debug.

3.7.4.1.4. Privileged access and unprivileged user access

Code can execute as privileged or unprivileged. Unprivileged execution limits resource access appropriate to the current

security state. Privileged execution has access to all resources available to the security state. Handler mode is always

privileged. Thread mode can be privileged or unprivileged.

3.7.4.2. Instruction set summary

The processor implements the following instruction from Armv8-M:

• All base instructions
• All instructions in the Main Extension
• All instructions in the Security Extension
• All instructions in the DSP Extension
• All single-precision instructions and double precision load/store instructions in the Floating-point Extension

For more information about Armv8-M instructions, see the Armv8-M Architecture Reference Manual.

3.7.4.3. Memory model

The processor contains a bus matrix that arbitrates instruction fetches and memory accesses from the processor core

between the external memory system and the internal System Control Space (SCS) and debug components.

Priority is usually given to the processor to keep debug accesses as non-intrusive as possible.

The system memory map is Armv8-M Main Extension compliant, and is common both to the debugger and processor

accesses.

The default memory map provides user and privileged access to all regions except for the Private Peripheral Bus (PPB).

The PPB space only allows privileged access.

The following table shows the default memory map. This is the memory map used when the included MPUs are

disabled. The attributes and permissions of all regions, except that targeting the NVIC and debug components, can be

modified using an implemented MPU.

| Address Range (inclusive) | Region | Interface |
| --- | --- | --- |
| 0x00000000 - 0x1FFFFFFF | Code | Instruction and data accesses. |
| 0x20000000 - 0x3FFFFFFF | SRAM | Instruction and data accesses. |
| 0x40000000 - 0x5FFFFFFF | Peripheral | Instruction and data accesses. Any attempt to execute instructions from the peripheral and external device region results in a MemManage fault. |
| 0x60000000 - 0x9FFFFFFF | External RAM | Instruction and data accesses. Any attempt to execute instructions from the peripheral and external device region results in a MemManage fault. |
| 0xA0000000 - 0xDFFFFFFF | External device | Instruction and data accesses. Any attempt to execute instructions from the peripheral and external device region results in a MemManage fault. |
| 0xE0000000 - 0xE00FFFFF | PPB | Reserved for system control and debug. Cannot be used for exception vector tables. Data accesses are either performed internally or on EPPB. Accesses in the range 0xE0000000 - 0xE0043FFF are handled within the processor. Accesses in the range 0xE0044000 - 0xE00FFFFF appear as APB transactions on the EPPB interface of the processor. Any attempt to execute instructions from the region results in a MemManage fault. |
| 0xE0100000 - 0xFFFFFFFF | Vendor_SYS | Partly reserved for future processor feature expansion. Any attempt to execute instructions from the region results in a MemManage fault. |

*Table 114. Default*

3.7. Cortex-M33 processor
132

RP2350 Datasheet

The internal Secure Attribution Unit (SAU) determines the security level associated with an address. Some internal

peripherals have memory-mapped registers in the PPB region which are banked between Secure and Non-secure state.

When the processor is in Secure state, software can access both the Secure and Non-secure versions of these

registers. The Non-secure versions are accessed using an aliased address.

For more information about the memory model, see the Armv8-M Architecture Reference Manual.

3.7.4.3.1. Private Peripheral Bus (PPB)

The Private Peripheral Bus (PPB) memory region provides access to internal and external processor resources.

The internal PPB provides access to:

• The System Control Space (SCS), including the Memory Protection Unit (MPU), Secure Attribution Unit (SAU), and

the Nested Vectored Interrupt Controller (NVIC).
• The Data Watchpoint and Trace (DWT) unit.
• The Breakpoint Unit (BPU).
• The Embedded Trace Macrocell (ETM).
• CoreSight Micro Trace Buffer (MTB).
• Cross Trigger Interface (CTI).
• The ROM table.

The external PPB (EPPB) provides access to implementation-specific external areas of the PPB memory map.

3.7.4.3.2. Unaligned accesses

The Cortex-M33 processor supports unaligned accesses. They are converted into two or more aligned AHB transactions

on the C-AHB or S-AHB master ports on the processor.

Unaligned support is only available for load/store singles (LDR, LDRH, STR, STRH, TBH) to addresses in Normal

memory. Load/store double and load/store multiple instructions already support word aligned accesses, but do not

permit other unaligned accesses, and generate a fault if this is attempted. Unaligned accesses in Device memory are

not permitted and fault. Unaligned accesses that cross memory map boundaries are architecturally UNPREDICTABLE.

3.7. Cortex-M33 processor
133

RP2350 Datasheet

NOTE

If CCR.UNALIGN_TRP for the current Security state is set, any unaligned accesses generate a fault.

3.7.4.4. Exclusive monitor

The Cortex-M33 processor implements a local exclusive monitor. The local monitor within the processor has been

constructed so that it does not hold any physical address, but instead treats any store-exclusive access as matching the

address of the previous load-exclusive. This means that the implemented exclusives reservation granule is the entire

memory address range. For more information about semaphores and the local exclusive monitor, see the Armv8-M

Architecture Reference Manual.

3.7.4.5. Processor core registers summary

The following table shows the processor core register set summary. Each of these registers is 32 bits wide. When the

Armv8-M Security Extension is included, some of the registers are banked. The Secure view of these registers is

available when the Cortex-M33 processor is in Secure state and the Non-secure view when Cortex-M33 processor is in

Non-secure state.

| Name | Description |
| --- | --- |
| R0-R12 | R0-R12 are general-purpose registers for data operations. |
| MSP (R13) | The Stack Pointer (SP) is register R13. In Thread mode, the CONTROL register indicates the stack pointer to use, Main Stack Pointer (MSP) or Process Stack Pointer (PSP). There are two MSP registers in the Cortex-M33 processor: MSP NS for the Non-secure state, and MSP S for the Secure _ _ state. |
| PSP (R13) | The Stack Pointer (SP) is register R13. In Thread mode, the CONTROL register indicates the stack pointer to use, Main Stack Pointer (MSP) or Process Stack Pointer (PSP). There are two PSP registers in the Cortex-M33 processor: PSP NS for the Non-secure state, and PSP S for the Secure _ _ state. |
| MSPLIM | The stack limit registers limit the extent to which the MSP and PSP registers can descend respectively. There are two MSPLIM registers in the Cortex-M33 processor: MSPLIM NS for the Non-secure state, and MSPLIM S for the _ _ Secure state. |
| PSPLIM | The stack limit registers limit the extent to which the MSP and PSP registers can descend respectively. There are two PSPLIM registers in the Cortex-M33 processor: PSPLIM NS for the Non-secure state, and PSPLIM S for the _ _ Secure state. |
| LR (R14) | The Link Register (LR) is register R14. It stores the return information for subroutines, function calls, and exceptions. |
| PC (R15) | The Program Counter (PC) is register R15. It contains the current program address. |
| PSR | The Program Status Register (PSR) combines the Application Program Status Register (APSR), Interrupt Program Status Register (IPSR), and Execution Program Status Register (EPSR). These registers provide different views of the PSR. |
| PRIMASK | The PRIMASK register prevents activation of exceptions with configurable priority. When the Armv8-M Security Extension is included, there are two PRIMASK registers in the Cortex-M33 processor: PRIMASK NS for the Non-secure state _ and PRIMASK S for the Secure state. _ |
| BASEPRI | The BASEPRI register defines the minimum priority for exception processing. There are two BASEPRI registers in the Cortex-M33 processor: BASEPRI NS for the Non-secure _ state, and BASEPRI S for the Secure state. _ |
| FAULTMASK | The FAULTMASK register prevents activation of all exceptions except for NON-MASKABLE INTERRUPT (NMI) and Secure HardFault. There are two FAULTMASK registers in the Cortex-M33 processor: FAULTMASK NS for the Non-secure _ state, and FAULTMASK S for the Secure state. _ |
| CONTROL | The CONTROL register controls the stack used, and optionally the privilege level, when the processor is in Thread mode. There are two CONTROL registers in the Cortex-M33 processor: CONTROL NS for the Non-secure state and _ CONTROL S for the Secure state. _ |

*Table 115. Processor core register set summary*

3.7. Cortex-M33 processor
134

RP2350 Datasheet

3.7.4.6. Exceptions

Exceptions are handled and prioritized by the processor and the NVIC. In addition to architecturally defined behaviour,

the processor implements advanced exception and interrupt handling that reduces interrupt latency and includes

implementation defined behaviour.

The processor core and the Nested Vectored Interrupt Controller (NVIC) together prioritize and handle all exceptions.

When handling exceptions:

• All exceptions are handled in Handler mode.
• Processor state is automatically stored to the stack on an exception, and automatically restored from the stack at

the end of the Interrupt Service Routine (ISR).
• The vector is fetched in parallel to the state saving, enabling efficient interrupt entry.

The processor supports tail-chaining that enables back-to-back interrupts without the overhead of state saving and

restoration.

Software can choose only to enable a subset of the configured number of interrupts, and can choose how many bits of

the configured priorities to use.

Exceptions can be specified as either Secure or Non-secure. When an exception occurs the processor switches to the

associated security state. The priority of Secure and Non-secure exceptions can be programmed independently. You

can deprioritise Non-secure configurable exceptions using the AIRCR.PRIS bit field to enable Secure interrupts to take

priority.

When taking and returning from an exception, the register state is always stored using the stack pointer associated with

the background security state. When taking a Non-secure exception from Secure state, all the register state is stacked

and then registers are cleared to prevent Secure data being available to the Non-secure handler. The vector base

3.7. Cortex-M33 processor
135

RP2350 Datasheet

address is banked between Secure and Non-secure state. VTOR_S contains the Secure vector base address, and VTOR_NS

contains the Non-secure vector base address. These registers can be programmed by software, and also initialized at

reset by the system.

NOTE

Vector table entries are compatible with interworking between Arm and Thumb instructions. This causes bit[0] of the

vector value to load into the Execution Program Status Register (EPSR) T-bit on exception entry. All populated

vectors in the vector table entries must have bit[0] set. Creating a table entry with bit[0] clear generates an INVSTATE

fault on the first instruction of the handler corresponding to this vector.

3.7.4.7. Security attribution and memory protection

Security attribution and memory protection in the processor is provided by the Security Attribution Unit (SAU) and the

Memory Protection Units (MPUs).

The SAU is a programmable unit that determines the security of an address. RP2350 includes 8 memory regions.

For instructions and data, the SAU returns the security attribute that is associated with the address.

For instructions, the attribute determines the allowable Security state of the processor when the instruction is executed.

It can also identify whether code at a Secure address can be called from Non-secure state.

For data, the attribute determines whether a memory address can be accessed from Non-secure state, and also whether

the external memory request is marked as Secure or Non-secure.

If a data access is made from Non-secure state to an address marked as Secure, then a SecureFault exception is taken

by the processor. If a data access is made from Secure state to an address marked as Non-secure, then the associated

memory access is marked as Non-secure.

The security level returned by the SAU is a combination of the region type defined in the internal SAU, if configured, and

the type that is returned on the associated Implementation Defined Attribution Unit (IDAU). If an address maps to

regions defined by both internal and external attribution units, the region of the highest security level is selected.

The register fields SAU_CTRL.EN and SAU_CTRL.ALLNS control the enable state of the SAU and the default security level when

the SAU is disabled. Both SAU_CTRL.EN and SAU_CTRL.ALLNS reset to zero disabling the SAU and setting all memory, apart

from some specific regions in the PPB space to Secure level. If the SAU is not enabled, and SAU_CTRL.ALLNS is zero, then

the IDAU cannot set any regions of memory to a security level lower than Secure, for example Secure NSC or NS. If the

SAU is enabled, then SAU_CTRL.ALLNS does not affect the Security level of memory.

RP2350 supports the Armv8-M Protected Memory System Architecture (PMSA). The MPU provides full support for:

• protection regions
• access permissions
• exporting memory attributes to the system

MPU mismatches and permission violations invoke the MemManage handler. For more information, see the Armv8-M

Architecture Reference Manual.

You can use the MPU to:

• enforce privilege rules
• separate processes
• manage memory attributes

The MPU supports 16 memory regions: 8 secure and 8 non-secure. The MPU is banked between Secure and Non-secure

states. The number of regions in the Secure and Non-secure MPU can be configured independently and each can be

programmed to protect memory for the associated Security state.

3.7. Cortex-M33 processor
136

RP2350 Datasheet

3.7.4.8. External coprocessors

The external coprocessor interface:

• Supports low-latency data transfer from the processor to and from the accelerator components.
• Has a sustained bandwidth up to twice of the processor memory interface.

The following instruction types are supported:

• Register transfer from the Cortex-M33 processor to the coprocessor MCR, MCRR, MCR2, MCRR2.
• Register transfer from the coprocessor to the Cortex-M33 processor MRC, MRRC, MRC2, MRRC2.
• Data processing instructions CDP, CDP2.

NOTE

The regular and extension forms of the coprocessor instructions for example, MCR and MCRR2, have the same

functionality but different encodings. The MRC and MRC2 instructions support the transfer of APSR.NZVC flags when the

processor register field is set to PC, for example Rt == 0xF.

3.7.4.8.1. Restrictions

The following restrictions apply when to coprocessor instructions:

• The LDC(2) or STC(2) instructions are not supported. If these are included in software with the <coproc> field set to a

value between 0-7 and the coprocessor is present and enabled in the appropriate fields in the CPACR/NSACR registers,

the Cortex-M33 processor always attempts to take an Undefined instruction (UNDEFINSTR) UsageFault exception.
• The processor register fields for data transfer instructions must not include the stack pointer (Rt == 0xD), this

encoding is UNPREDICTABLE in the Armv8-M architecture and results in an Undefined instruction (UNDEFINSTR)

UsageFault exception in the CPACR/NSACR registers.
• If any coprocessor instruction is executed when the corresponding coprocessor is disabled in the CPACR/NSACR

register, the Cortex-M33 processor always attempts to take a No coprocessor (NOCP) UsageFault exception.

3.7.4.8.2. Data transfer rates

The following table shows the ideal data transfer rates for the coprocessor interface. This means that the coprocessor

responds immediately to an instruction. The ideal data transfer rates are sustainable if the corresponding coprocessor

instructions are executed consecutively.

The following instructions have the following data transfer rates:

MCR, MCR2 (Processor to coprocessor)

32 bits per cycle

MRC, MRC2 (Coprocessor to processor)

32 bits per cycle

MCRR, MCRR2 (Processor to coprocessor)

64 bits per cycle

MRRC, MRRC2 (Coprocessor to processor)

64 bits per cycle

3.7.4.9. Execution timing

This section describes the execution time of various Cortex-M33 instructions. The results are based on measurements

3.7. Cortex-M33 processor
137

RP2350 Datasheet

of a limited and non-exceptional set of examples of the more common instructions and hence may not correctly cover

some more unusual situations.

These measurements were taken with the following conditions:

• only one core is running
• there are no cache misses (in particular, no XIP cache misses)
• there there is no active DMA

Any of the above conditions can affect the timing of instruction fetch as well as of load and store operations. See the

description of the bus fabric elsewhere in this datasheet for information on possible contention for access to memory.

3.7.4.9.1. Result delays

Some instructions generate results with a two-cycle latency. Using such a result as a source operand for a subsequent

instruction incurs a one-cycle result-use penalty. Most of the input values of any instruction count as source operands,

including:

• any source register in a data processing (ALU) instruction
• any registers used in address generation by an LDR or LDM (including R13 in the case of POP)
• any registers used in address generation (but not those to be stored) by a STR or STM (including R13 in the case of

PUSH).

The following example shows a load followed by a data-processing instruction, an instruction sequence which incurs

this penalty:

```c
LDR R0,[R1]
ADD R1,R0,R2
```

The following instructions generate results with a two-cycle latency:

• the destination register arising from some non-simple shifts in certain data-processing instructions (specified in

more detail below)
• the destination register or registers of a multiply instruction
• the destination register of an LDR
• the last register in the register list of an LDM or POP unless that register is R15

Using results of the above instructions as a source operand for another instruction incurs a one-cycle penalty between

the operations.

3.7.4.9.2. Simple arithmetic and logical instructions

Most data processing instructions execute in a single cycle. Some complex operations (including those listed above

and SEL) incur a result-use penalty.

Complex operations meet at least one of the following criteria:

• a shifted operand where the shift is not LSL#0, LSL#1, LSL#2 or LSL#3
• an immediate operand which entails a shift (i.e., not of the form 0x000000XY, 0xXYXYXYXY, 0x00XY00XY or 0xXY00XY00)

When a complex instruction has the -S suffix to set flags, the one-cycle penalty is always incurred, even if the next

instruction does not depend on those flag values.

The following operations do not incur a penalty:

3.7. Cortex-M33 processor
138

RP2350 Datasheet

```c
AND R0,R1,R2,LSL#4
MOV R3,R4
```

However, the following operations do incur a penalty:

```c
AND R0,R1,R2,LSL#4
MOV R3,R0
ANDS R0,R1,R2,LSL#4
MOV R3,R4
```

ADD and SUB are available in variants with a 12-bit plain immediate operand. These do not incur a penalty.

MOV and MOVS with an immediate operand, including MOV with a 16-bit plain immediate operand, do not incur a result-use

penalty.

Despite their similarity to logical operations with a shifted operand, UBFX, SBFX and BFI do not incur a result-use penalty.

Simple shift instructions (LSL, LSR, ASR, and ROR, with the shift amount specified either as an immediate constant or in a

register) take one cycle with no result-use penalty.

3.7.4.9.3. Multiply instructions

Multiply and multiply-accumulate instructions execute in a single cycle, but all have a result delay of one cycle.

However, the special case of using the result of a multiply instruction as the accumulate input to a following multiply-

accumulate instruction does not incur a one-cycle penalty. As a result, repeated multiply-accumulate operations can run

at one per cycle, assuming all of the following conditions hold:

• the operations accumulate into the same register or register pair
• the multiplier and multiplicand operands come from other registers

Sequences such as the following can execute one instruction per cycle:

```c
MLA R0,R1,R2,R3
MLA R3,R1,R2,R0
MLA R0,R1,R2,R3
MLA R3,R1,R2,R0
...
UMLAL R0,R1,R4,R5
UMLAL R2,R3,R6,R7
UMLAL R0,R1,R4,R5
UMLAL R2,R3,R6,R7
...
```

The multiplier requires its multiply operands on cycle n, requires its accumulate operand (if any) on cycle n+1, and

makes its result available on cycle n+2.

As a further example, the following sequence completes in 4 cycles:

3.7. Cortex-M33 processor
139

RP2350 Datasheet

```c
ADD R2,R0,R1,LSL#23
MLA R3,R4,R5,R2
MOV R6,R3
```

The ADD would normally incur a one-cycle result-use penalty, but in this case its result is not needed until the second

cycle of the multiply-accumulate operation, eliminating the penalty.

3.7.4.9.4. Divide instructions

Let n be the difference between the bit positions of the most significant ones in the absolute value of the dividend and

the absolute value of the divisor. If n is negative (in which case the result will be zero), division takes 2 cycles.

Otherwise, division takes 4+n/4 cycles, rounded down.

Using the result of division as input for the next instruction incurs a a one-cycle result-use penalty.

3.7.4.9.5. Register loads (LDR and LDM)

Loads execute in one cycle per register, plus a possible one-cycle result delay. Loads can slow down if the addressed

memory is not able to accept the read request immediately, for example because of contention with instruction

prefetch.

From the point of view of result delays, any register used in address generation counts as a source operand. For

examples, see Table 116.

| Instruction | Source Operand | Not a Source Operand |
| --- | --- | --- |
| LDR R0,[R5,R6] | R5, R6 | R0 |
| LDMIA R7,{R0-R3} | R7 | R0, R1, R2, R3 |

*Table 116. Load instruction source operand examples*

There is one cycle of result delay associated with the destination register of an LDR and with the last register in the

register list of an LDM or POP. For example, R7 has one cycle of result delay both in LDR R7,[R5,R6] and in LDMIA R0,{R1-R7}.

The latter case incurs no result delay associated with R1 to R6.

Loading R15 does not cause any result delay; however, extra cycles will be taken as described in Section 3.7.4.9.7.

3.7.4.9.6. Register stores (STR and STM)

Stores, including those which depend on the contents of three different registers such as STR R0,[R1,R2,LSL#2], execute

in one cycle. Like loads, stores can slow down if the addressed memory is not able to accept the request immediately.

The registers involved in address generation, but not the register or registers being stored, count as source operands

from the point of view of result delays. For examples, see Table 117.

| Instruction | Source Operand | Not a Source Operand |
| --- | --- | --- |
| STR R5,[R6,R7] | R6, R7 | R5 |
| STMFD R5!,{R0-R3} | R5 | R0, R1, R2, R3 |

*Table 117. Register stores source operand examples*

3.7.4.9.7. Branches

This section covers any instruction that can change R15, including the following:

• MOV R15,Rx

3.7. Cortex-M33 processor
140

RP2350 Datasheet

• BNE
• BL
• BX
• LDR R15,…
• POP {…,R15}

When a branch arises from a load (LDR R15,…, LDMxx Rx,{…,R15}, or POP {…,R15}), the basic time for the instruction is that

taken by the load instruction itself, as described in Section 3.7.4.9.5.

For other instructions that can change R15 (MOV R15,Rx, B<cond>, BL, BX), the basic time for the instruction is zero.

The total time required for a branch that does occur is the basic time + 2 + L + U- ( K & F ) cycles, and the time required for

a branch that does not occur is the basic time + 1 - F cycles, where L, U, F, K are each 0 or 1 as described below:

L

1 when the branch arises from a load (LDR R15,[R6], LDMIA R13,{R0-R3,R15}, and so on); 0 otherwise.

U

1 when the all of following conditions are true:

• the target address of the branch is not word-aligned
• the instruction at that address is 32 bits long
• the instruction executed immediately prior to the branch is not POP or PUSH

If any of the above conditions are not true, U is 0.

F

indicates when the branch can be dual issued (or "folded") with the previous instruction, PrevInst (the instruction

executed immediately prior to the branch). F is 1 when all of the following conditions are true:

• the branch instruction is B, B<cond>, or BX R14 (but not BX to any other register or MOV R15,R14)
• PrevInst is 16 bits long
• PrevInst was executed sequentially (i.e., not itself branched to), or PrevInst is word-aligned
• PrevInst itself was not itself folded with a previous instruction

If any of the above conditions are not true, F is 0.

K

1 when it is known that the branch will execute prior to executing PrevInst; 0 otherwise. In other words, K is 1 unless

the branch is conditional and PrevInst sets the flags.

For example, the following delay loop takes 299 cycles:

```c
10000002: MOVS R5,#100
10000004: SUBS R5,R5,#1
10000006: BNE 0x10000004
```

Those cycles come from the following timings:

• 1 cycle for MOVS R5,#100
• 1 cycle for each SUBS R5,R5,#1
• 2 cycles each for the first 99 BNE instructions (L=U=0, F=1, K=0)
• 0 cycles for the last, non-taken, BNE (L=U=0, F=1, K=0, using the formula 1-F)

At a different alignment, the same delay loop takes 300 cycles:

3.7. Cortex-M33 processor
141

RP2350 Datasheet

```c
10000000: MOVS R5,#100
10000002: SUBS R5,R5,#1
10000004: BNE 0x10000002
```

Those cycles come from the following timings:

• 1 cycle for MOVS R5,#100;
• 1 cycle for each SUBS R5,R5,#1;
• 2 cycles for the first BNE (L=U=0, F=1, K=0);
• 2 cycles each for the next 98 BNE instructions (L=U=0, F=K=0);
• 1 cycle for the last, non-taken, BNE (L=U=0, F=K=0, using the formula 1-F).

This longer delay loop also takes 300 cycles:

```c
10000000: MOVS R5,#100
10000002: SUBS R5,R5,#1
10000004: MOV R1,R2
10000006: BNE 0x10000002
```

Those cycles come from the following timings:

• 1 cycle for MOVS R5,#100;
• 1 cycle for each SUBS R5,R5,#1;
• 1 cycle for each MOV R1,R2;
• 1 cycle each for the first 99 BNE instructions (L=U=0, F=K=1);
• 0 cycles for the last, non-taken, BNE (L=U=0, F=K=1; using the formula 1-F).

This example illustrates that, if you can contrive to place an instruction between the loop-end test and the branch, it can

potentially have zero net cost in execution time.

Another optimisation is to try to ensure that a branch target is either word-aligned or is a 16-bit instruction. Any

instruction following a BL can be considered a branch target from this point of view as it is branched to by the return

instruction from the subroutine.

If space and cache permit, unrolling loops and inlining subroutines avoids the branch cost altogether.

A sequence of branches not taken will alternately take 0 cycles and 1 cycle. That is the same as a sequence of NOP

instructions, which can also be folded. However, this is not the same as sequence of instructions in an IT block that fail

their condition.

3.7.4.9.8. IT (if-then) blocks

Instructions within an IT block whose condition fails execute in one cycle.

Most instructions within an IT block whose condition succeeds take the number of cycles they would have taken in their

normal, unconditional state.

3.7.4.9.9. Dual issue

When a 16-bit instruction follows a NOP instruction (opcode 0xBF00, not 0x46C0), the instructions are folded, executing the

NOP in zero cycles. In some situations, this can help align a branch target to a word-aligned address without an

3.7. Cortex-M33 processor
142

RP2350 Datasheet

execution-time penalty.

When a 16-bit opcode follows an IT instruction, the IT instruction executes in zero cycles.

The Cortex-M33 core folds a NOP with the previous instruction (PrevInst) if all of the following conditions are true:

• PrevInst is 16 bits long
• PrevInst was executed sequentially (not itself branched to) or PrevInst is word-aligned
• PrevInst was not itself folded with a previous instruction

Branches not taken are in this sense similar to NOP instructions: they can be folded according to the same rule. For

further detail on when taken and not taken branches are folded, see Section 3.7.4.9.7.

When two multi-cycle instructions are folded, at most one cycle can overlap between the instructions.

3.7.4.9.10. Floating-point coprocessor operations

This section describes operations involving the single-precision floating-point coprocessor (FPU). For timings relating to

the GPIO coprocessor, the double-precision coprocessor, and the redundancy coprocessor, see Section 3.7.4.9.11 and

the detailed descriptions of those coprocessors elsewhere in this document.

Issuing a floating-point instruction occupies the integer core for one cycle. After that, the integer core can proceed with

other non-FPU operations without interruption.

Attempting to issue another FPU instruction stalls execution until the FPU is ready to accept the FPU instruction.

The following list details the timings of various FPU instructions:

• VADD.F32, VSUB.F32 and VMUL.F32 can execute in one cycle, but have an additional cycle of result delay. As a result, the

following example sequence executes at two cycles per instruction:

```c
VADD.F32 s0,s0,s2
VADD.F32 s0,s0,s3
VADD.F32 s0,s0,s4
VADD.F32 s0,s0,s5
...
```

The following interleaved example, however, executes at one cycle per instruction:

```c
VADD.F32 s0,s0,s2
VADD.F32 s1,s1,s3
VADD.F32 s0,s0,s4
VADD.F32 s1,s1,s5
...
```

Furthermore, you can interleave VADD.F32, VSUB.F32 and VMUL.F32 instructions arbitrarily to execute in one cycle, as

long as no instruction depends on the result of its predecessor.
• VMLA.F32 and VFMA.F32 occupy the FPU for 3 cycles, plus one cycle of result delay.

However, consecutive VMLA.F32 or VFMA.F32 instructions accumulating into the same register can run at one

instruction every three cycles.
• When the work can be interleaved, separate VMUL.F32 and VADD.F32 instructions are faster than a single VMLA.F32

instruction.
• VDIV.F32 and VSQRT.F32 occupy the FPU for 14 cycles, plus one cycle of result delay.
• VMOV.F32 Sx,Ry (move one word from integer register to coprocessor) takes one cycle.

3.7. Cortex-M33 processor
143

RP2350 Datasheet

• VMOV.F32 Rx,Sy (move one word from coprocessor to integer register) takes one cycle plus one cycle of result delay.
• VMOV.F32 Sx,Sy (move one word between coprocessor registers) takes one cycle.
• VMOV.F64 Dx,Ry,Rz (move two words from integer registers to coprocessor) occupies the FPU for two cycles and the

integer core for one cycle.
• VMOV.F64 Rx,Ry,Dz (move two words from coprocessor to integer registers) occupies both the FPU and the integer

core for two cycles.

3.7.4.9.11. Other coprocessor operations

A coprocessor can stall an operation if it is not ready. For more information, see the documentation for the specific

coprocessor.

The following list details the timings of various coprocessor instructions:

• Assuming that no stalls occur, a CDP instruction takes one cycle.
• An MCR instruction (move one word from integer register to coprocessor) takes one cycle.
• An MRC instruction (move one word from coprocessor to integer register) takes one cycle, plus one cycle of result

delay.
• An MCRR instruction (move two words from integer registers to coprocessor) takes one cycle.
• An MRRC instruction (move two words from coprocessor to integer registers) takes one cycle, plus one cycle of

result delay.

3.7.4.9.12. Instruction fetch

Each Cortex-M33 core has separate instruction and data buses ("Harvard architecture"). Each core has a bandwidth to

memory of 32 bits per cycle. Since each instruction is at most 32 bits long, for sequential code the instruction

prefetcher has enough bandwidth to ensure that the processor core always has instructions.

In RP2350, contention can occur when the instruction and data buses attempt to access data stored in memory

connected to the same downstream port of the AHB5 crossbar. For example, code running from the main SRAM might

attempt to load a literal stored nearby. That load might conflict with an instruction prefetch to the same SRAM. To

reduce the chance of this conflict, the main SRAM is striped into banks across groups of four words: words at

addresses that are different modulo 16 are stored in different banks.

Since the prefetcher typically runs about two words (8 bytes) ahead of execution, that means that an instruction that

reads 8 (modulo 16) bytes ahead of itself is liable to result in a conflict. For example, the following instruction, which

reads 40 bytes ahead (because here PC means the address of the next instruction), can sometimes incur a penalty of

one cycle:

```c
LDR R8,[PC,#32] @ 32-bit instruction
```

3.7.4.10. Debug

Cortex-M33 debug functionality includes processor halt, single-step, processor core register access, Vector Catch,

unlimited software breakpoints, and full system memory access.

The processor also includes support for hardware breakpoints and watchpoints configured during implementation:

• A breakpoint unit supporting eight instruction comparators
• A watchpoint unit supporting four data watchpoint comparators

The Cortex-M33 processor supports system level debug authentication to control access from a debugger to resources

3.7. Cortex-M33 processor
144

RP2350 Datasheet

and memory. Authentication via the Armv8-M Security Extension can be used to allow a debugger full access to Non-

secure code and data without exposing any Secure information.

The processor implementation can be partitioned to place the debug components in a separate power domain from the

processor core and NVIC.

All debug registers are accessible by the D-AHB interface.

For more information, see the Armv8-M Architecture Reference Manual.

3.7.4.11. Data Watchpoint and Trace unit (DWT)

The DWT is a full configuration, containing four comparators (DWT_COMP0 to DWT_COMP3). These comparators support the

following features:

• Hardware watchpoint support
• Hardware trace packet support
• CMPMATCH support for ETM/MTB/CTI triggers
• Cycle counter matching support (DWT_COMP0 only)
• Instruction address matching support
• Data address matching support
• Data value matching support (DWT_COMP1 only in a reduced DWT, DWT_COMP3 only in a Full DWT)
• Linked/limit matching support (DWT_COMP1 and DWT_COMP3 only)

The DWT contains counters for:

• Cycles (DWT_CYCCNT.CYCCNT)
• Folded Instructions (FOLDCNT)
• Additional cycles required to execute all load/store instructions (LSUCNT)
• Processor sleep cycles (SLEEPCNT)
• Additional cycles required to execute multi-cycle instructions and instruction fetch stalls (CPICNT)
• Cycles spent in exception processing (EXCCNT)

Before using DWT, set the DEMCR.TRCENA bit to 1.

The DWT provides periodic requests for protocol synchronization to the ITM and the TPIU.

3.7.4.12. Cross Trigger Interface (CTI)

The CTI enables the debug logic, MTB, and ETM to interact with each other and with other CoreSight components. This

is called cross triggering. For example, you can configure the CTI to generate an interrupt when the ETM trigger event

occurs or to start tracing when a DWT comparator match is detected.

The following figure shows the debug system components and the available trigger inputs and trigger outputs:

Figure 15 shows the components of the debug system.

3.7. Cortex-M33 processor
145

RP2350 Datasheet

*Figure 15. Debug system components*

The following table shows how the CTI trigger inputs are connected to the Cortex-M33 processor:

| Signal | Description | Connection | Acknowledge, handshake |
| --- | --- | --- | --- |
| CTITRIGIN[7] |  | ETM to CTI | Pulsed |
| CTITRIGIN[6] |  | ETM to CTI | Pulsed |
| CTITRIGIN[5] | ETM Event Output 1 | ETM to CTI | Pulsed |
| CTITRIGIN[4] | ETM Event Output 0 or Comparator Output 3 | ETM/Processor to CTI | Pulsed |
| CTITRIGIN[3] | DWT Comparator Output 2 | Processor to CTI | Pulsed |
| CTITRIGIN[2] | DWT Comparator Output 1 | Processor to CTI | Pulsed |
| CTITRIGIN[1] | DWT Comparator Output 0 | Processor to CTI | Pulsed |
| CTITRIGIN[0] | Processor Halted | Processor to CTI | Pulsed |

*Table 118. Trigger*

The following table shows how the CTI trigger outputs are connected to the processor and ETM:

| Signal | Description | Connection | Acknowledge, handshake |
| --- | --- | --- | --- |
| CTITRIGOUT[ 7] | ETM Event Input 3 | CTI to ETM | Pulsed |
| CTITRIGOUT[ 6] | ETM Event Input 2 | CTI to ETM | Pulsed |
| CTITRIGOUT[ 5] | ETM Event Input 1 or MTB Trace stop | CTI to ETM or MTB | Pulsed |
| CTITRIGOUT[ 4] | ETM Event Input 1 or MTB Trace start | CTI to ETM or MTB | Pulsed |
| CTITRIGOUT[ 3] | Interrupt request 1 | CTI to system | Acknowledged by writing to the CTIINTACK register in ISR |
| CTITRIGOUT[ 2] | Interrupt request 0 | CTI to system | Acknowledged by writing to the CTIINTACK register in ISR |
| CTITRIGOUT[ 1] | Processor Restart | CTI to Processor | Processor Restarted |
| CTITRIGOUT[ 0] | Processor debug request | CTI to Processor | Acknowledged by the debugger writing to the CTIINTACK register |

*Table 119. Trigger*

3.7. Cortex-M33 processor
146

RP2350 Datasheet

After the processor is halted using CTI Trigger Output 0, the Processor Debug Request signal remains asserted. The

debugger must write to CTIINTACK to clear the halting request before restarting the processor.

After asserting an interrupt using the CTI Trigger Output 1 or 2, the Interrupt Service Routine (ISR) must clear the

interrupt request by writing to the CTI Interrupt Acknowledge, CTIINTACK.

Interrupt requests from the CTI to the system are only asserted when invasive debug is enabled in the processor.

3.7.4.12.1. CTI programmers model

The following table shows the CTI programmable registers, with address offset, type, and reset value for each register.

See the Arm CoreSightTM SoC-400 Technical Reference Manual for register descriptions.

| Address offset | Name | Type | Reset value | Description |
| --- | --- | --- | --- | --- |
| 0xE0042000 | CTICONTROL | RW | 0x00000000 | CTI Control Register |
| 0xE0042010 | CTIINTACK | WO | UNKNOWN | CTI Interrupt Acknowledge Register |
| 0xE0042014 | CTIAPPSET | RW | 0x00000000 | CTI Application Trigger Set Register |
| 0xE0042018 | CTIAPPCLEAR | RW | 0x00000000 | CTI Application Trigger Clear Register |
| 0xE004201C | CTIAPPPULSE | WO | UNKNOWN | CTI Application Pulse Register |
| 0xE0042020-0xE004203C | CTIINEN[7:0] | RW | 0x00000000 | CTI Trigger to Channel Enable Registers |
| 0xE00420A0-0xE00420BC | CTIOUTEN[7:0] | RW | 0x00000000 | CTI Channel to Trigger Enable Registers |
| 0xE0042130 | CTITRIGINSTATUS | RO | 0x00000000 | CTI Trigger In Status Register |
| 0xE0042134 | CTITRIGOUTSTATUS | RO | 0x00000000 | CTI Trigger Out Status Register |
| 0xE0042138 | CTICHINSTATUS | RO | 0x00000000 | CTI Channel In Status Register |
| 0xE0042140 | CTIGATE | RW | 0x0000000F | Enable CTI Channel Gate Register |
| 0xE0042144 | ASICCTL | RW | 0x00000000 | External Multiplexer Control Register |
| 0xE0042EE4 | ITCHOUT | WO | UNKNOWN | Integration Test Channel Output Register |
| 0xE0042EE8 | ITTRIGOUT | WO | UNKNOWN | Integration Test Trigger Output Register |
| 0xE0042EF4 | ITCHIN | RO | 0x00000000 | Integration Test Channel Input Register |
| 0xE0042F00 | ITCTRL | RW | 0x00000000 | Integration Mode Control Register |
| 0xE0042FC8 | DEVID | RO | 0x00040800 | Device Configuration Register |
| 0xE0042FBC | DEVARCH | RO | 0x47701A14 | Device Architecture Register |
| 0xE0042FCC | DEVTYPE | RO | 0x00000014 | Device Type Identifier Register |
| 0xE0042FD0 | PIDR4 | RO | 0x00000004 | Peripheral ID4 Register |
| 0xE0042FD4 | PIDR5 | RO | 0x00000000 | Peripheral ID5 Register |
| 0xE0042FD8 | PIDR6 | RO | 0x00000000 | Peripheral ID6 Register |
| 0xE0042FDC | PIDR7 | RO | 0x00000000 | Peripheral ID7 Register |
| 0xE0042FE0 | PIDR0 | RO | 0x00000021 | Peripheral ID0 Register |
| 0xE0042FE4 | PIDR1 | RO | 0x000000BD | Peripheral ID1 Register |
| 0xE0042FE8 | PIDR2 | RO | 0x0000000B | Peripheral ID2 Register |
| 0xE0042FEC | PIDR3 | RO | 0x00000001 | Peripheral ID3 Register |
| 0xE0042FF0 | CIDR0 | RO | 0x0000000D | Component ID0 Register |
| 0xE0042FF4 | CIDR1 | RO | 0x00000090 | Component ID1 Register |
| 0xE0042FF8 | CIDR2 | RO | 0x00000005 | Component ID2 Register |
| 0xE0042FFC | CIDR3 | RO | 0x000000B1 | Component ID3 Register |

*Table 120. Cortex-M33*

3.7. Cortex-M33 processor
147

RP2350 Datasheet

## Embedded Images

![img_p0147_00.png](images/img_p0147_00.png)


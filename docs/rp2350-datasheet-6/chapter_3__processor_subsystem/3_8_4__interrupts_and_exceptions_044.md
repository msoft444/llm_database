---
source_pdf: rp2350-datasheet-6.pdf
repository: llm_database
chapter: Chapter 3. Processor subsystem
section: 3.8.4. Interrupts and exceptions
pages: 283-286
type: technical_spec
generated_at: 2026-03-01T05:39:21.783275+00:00
---

# 3.8.4. Interrupts and exceptions

3.8.4. Interrupts and exceptions

In the RISC-V privileged ISA manual, a trap refers to either an interrupt or an exception:

Interrupt

A signal from outside the processor requests that it temporarily abandons its current task to deal with some

system-level event. The processor responds by transferring control to an interrupt handler function.

Exception

An instruction encounters a condition that prevents that instruction from completing normally. The processor

transfers control to an exception handler function to deal with the exceptional condition before it can resume

execution.

The two are closely related, and they are collectively referred to as traps to avoid stating everything twice.

Hardware performs the following steps automatically and atomically when entering a trap:

1. Save the address of the interrupted or excepting instruction to MEPC

2. Set the MSB of MCAUSE to indicate the cause is an interrupt, or clear it to indicate an exception

3. Write the detailed trap cause to the LSBs of the MCAUSE register

4. Save the current privilege level to MSTATUS.MPP

5. Set the privilege to M-mode (note Hazard3 does not implement S-mode)

6. Save the current value of MSTATUS.MIE to MSTATUS.MPIE

7. Disable interrupts by clearing MSTATUS.MIE

8. Jump to the correct offset from MTVEC depending on the trap cause

NOTE

The above sequence of events is standard and is also described in the RISC-V Privileged ISA Manual. See Section

3.8.1.1 for a list of links to RISC-V specifications.

All earlier instructions than the one pointed to by MEPC execute normally, and their effects are visible to the trap

handler. These earlier instructions are not affected by the exception or interrupt. On the other hand the instruction

pointed to by MEPC, and all later instructions, does not execute before entering the trap handler. These instructions

have no visible side effects, with the possible exception of load/store fault exceptions, where the bus fault itself may

have observable effects on the bus or peripheral.

Expanding on the MEPC behaviour in architectural terms, all traps are precise, meaning there exists some point in

program order where the trap handler observes all earlier instructions to have retired and all later instructions to have

not. The MEPC register indicates this point. All exceptions are also synchronous, meaning there is a particular

instruction that originated the trap, and the trap architecturally takes place in between that instruction and its

predecessors in program order.

M-mode software executes an mret instruction to return to the interrupted or excepting instruction at the end of a

handler. This largely reverses the process of entering the trap:

1. Restore core privilege level to the value of MSTATUS.MPP

2. Write 0 (U-mode) to MSTATUS.MPP

3.8. Hazard3 processor
282

RP2350 Datasheet

3. Restore MSTATUS.MIE from MSTATUS.MPIE

4. Write 1 to MSTATUS.MPIE

5. Jump to the address in MEPC.

Often, the values restored on exit are exactly those values saved on entry. However this need not be the case, as all

CSRs mentioned above are read/writable by M-mode software at any time. Hand-manipulating the trap handling CSRs is

useful for low-level OS operations such as context switching, or to make exception handlers return to the instruction

after the trap point by incrementing MEPC before return. You can execute an mret without any prior trap, for example

when entering U-mode code from M-mode for the first time.

Hardware does not save or restore any other registers. In particular, it does not save the core GPRs, and software is

responsible for ensuring the execution of the handler does not perturb the foreground context. For an interrupt, this may

mean saving the core registers on the interruptee’s stack, or using the MSCRATCH CSR to swap the stack pointer before

saving registers on a dedicated interrupt stack. For a fatal exception this may be unimportant, as there is no

requirement for the handler to return.

3.8.4.1. Exceptions

Exceptions occur for a variety of reasons. MCAUSE indicates the specific reason for the latest exception:

| Cause | Meaning |
| --- | --- |
| 0x0 | Instruction alignment: Does not occur on RP2350, because 16-bit compressed instructions are implemented, and it is impossible to jump to a byte-aligned address. |
| 0x1 | Instruction fetch fault: Attempted to fetch from an address that does not support instruction fetch (like APB/AHB peripherals on RP2350), or lacks PMP execute permission, or is forbidden by ACCESSCTRL, or returned a fault from the memory device itself. |
| 0x2 | Illegal instruction: Encountered an instruction that was not a valid RISC-V opcode implemented by this processor, or attempted to access a nonexistent CSR, or attempted to execute a privileged instruction or access a privileged CSR without sufficient privilege. |
| 0x3 | Breakpoint: An ebreak or c.ebreak instruction was executed, and no external debug host caught it ( DCSR.EBREAKM or DCSR.EBREAKU was not set). |
| 0x4 | Load alignment: Attempted to load from an address that was not a multiple of access size. |
| 0x5 | Load fault: Attempted to load from an address that does not exist, or lacks PMP read permissions, or is forbidden by ACCESSCTRL, or returned a fault from a peripheral. |
| 0x6 | Store/AMO alignment: Attempted to write to an address that was not a multiple of access size. |
| 0x7 | Store/AMO fault: Attempted to write to an address that does not exist, or lacks PMP write permissions, or is forbidden by ACCESSCTRL, or returned a fault from a peripheral. Also raised when attempting an AMO on an address that does not support AHB5 exclusives. |
| 0x8 | An ecall instruction was executed in U-mode. |
| 0xb | An ecall instruction was executed in M-mode. |

Exceptions jump to exactly the address of MTVEC, no matter the cause and no matter whether vectoring is enabled.

The MSTATUS.MIE global interrupt enable does not affect exception entry. You can still take an exception and trap into

the exception handler when exceptions are disabled.

Returning from an exception will jump to MEPC, which hardware sets to the address of the excepting instruction before

entering the exception handler. This means by default you will return to the exact same instruction that caused the

exception. When emulating illegal instructions, you should increment mepc before returning, so that execution resumes

after the problematic instruction.

Hazard3 hardwires mtval to zero. To emulate a misaligned load/store instruction you must decode the instruction and

3.8. Hazard3 processor
283

RP2350 Datasheet

read the spilled register state to calculate the address, and to emulate an illegal instruction you must read the

instruction bits from memory yourself by dereferencing mepc.

3.8.4.2. Interrupts

Hazard3 implements the standard RISC-V interrupt scheme with a single external interrupt routed to MIP.MEIP, and the

standard timer and soft interrupts routed to MTIP and MSIP. An interrupt controller such as a standard RISC-V PLIC can

be integrated externally to route multiple interrupts through to the single external interrupt line. Alternatively, the

Hazard3 interrupt controller (see Xh3irq extension, Section 3.8.6.1) multiplexes multiple external interrupts onto

MIP.MEIP in such a way that interrupts can efficiently pre-empt one another, with configurable dynamic priority per

interrupt.

RP2350 configures Hazard3 with the Xh3irq interrupt controller, with 52 external interrupt lines and 16 levels of pre-

emption priority. The IRQ numbers for the system-level interrupts, documented in Section 3.2, are the same on both Arm

and RISC-V.

The core enters an interrupt when all of the following are true:

• MSTATUS.MIE is set
• An interrupt pending bit in the standard MIP CSR is set
• The matching interrupt enable in the standard MIE CSR is also set

When vectoring is disabled (LSB of MTVEC is clear), interrupts transfer control directly to the address indicated by mtvec.

Setting the LSB enables vectoring: interrupts transfer control to the address mtvec + 4 * cause, where the interrupt cause

is one of:

• meip: cause = 11
• mtip: cause = 7
• msip: cause = 3

The pointer written to mtvec must be word-aligned (4 bytes). Additionally, when vectoring is enabled, it must be aligned

to the size of the table, rounded up to a power of two. This works out to 64-byte alignment. On RP2350, mtvec is fully

writable except for bit 1, which is hardwired to zero as it is only used for additional vectoring modes not supported by

Hazard3.

When multiple interrupts are active, hardware picks one to enter, in the order meip > msip > mtip. (This is not quite the

same order as the cause values.)

3.8.4.2.1. RISC-V interrupt signals

The standard timer interrupt MIP.MTIP connects to the RISC-V platform timer in the SIO subsystem (Section 3.1.8). This

is a 64-bit timer with a per-core 64-bit comparison value. The interrupt is asserted whenever the timer is greater than or

equal to the comparison value, and de-asserts automatically when less than. The same interrupt signal also appears in

the system-level IRQs, as SIO_IRQ_MTIMECMP (IRQ 29). The timer is a standard RISC-V peripheral, often used by operating

systems to generate context switch interrupts.

The standard software interrupt MIP.MSIP connects to the RISCV_SOFTIRQ register in the SIO subsystem. The register

has a single bit per hart, which asserts the soft IRQ interrupt to that hart. This can be used to interrupt the other hart, or

to interrupt yourself as though the other hart had interrupted you, which can help to make handler code more

symmetric. On RP2350 there is a one-to-one correspondence between harts and cores, so you could equivalently say

there is one soft IRQ per core.

Hazard3’s internal interrupt controller drives the MIP.MEIP external interrupt pending bit based on its internal state and

the system-level interrupt signals, to transfer control to the interrupt vector when it is both safe and necessary. Section

3.8.6.1 describes the Xh3irq interrupt controller in depth.

3.8. Hazard3 processor
284

RP2350 Datasheet

3.8.4.2.2. Interrupt calling convention

The default SDK hardware_irq library expects function pointers registered for system-level IRQs to be normal C functions.

There must be no 
__attribute__((interrupt)) on an interrupt handler passed into functions such as

set_exclusive_irq_handler(). This is an API detail that is consistent across all architectures supported by the SDK. Using

regular C calling convention is also efficient under heavy interrupt load, because the cost of saving/restoring all caller

save and temporary registers can be amortised over multiple interrupt handlers due to tail sharing, and a save triggered

by a low-priority IRQ can be taken over by a high-priority IRQ that asserted during the save.

Conversely, 
handlers 
registered 
for 
the 
standard 
RISC-V 
mtip 
and 
msip 
interrupts 
via 
the 
SDK

irq_set_riscv_vector_handler() function must be __attribute__((interrupt)). In terms of the generated code, this means

they should use save-as-you-go calling convention, and end with an mret. These interrupts are entered directly by the

hardware without any intermediate dispatch code.

As software is responsible for the dispatch to individual system interrupt handlers from the meip vector, it is possible to

support other interrupt calling conventions by supplying a different implementation for the dispatch.

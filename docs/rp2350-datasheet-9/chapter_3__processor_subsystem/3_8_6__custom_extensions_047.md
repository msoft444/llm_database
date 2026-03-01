---
source_pdf: rp2350-datasheet-9.pdf
repository: llm_database
chapter: Chapter 3. Processor subsystem
section: 3.8.6. Custom extensions
pages: 287-297
type: technical_spec
generated_at: 2026-03-01T11:44:31.548839+00:00
---

# 3.8.6. Custom extensions

3.8.6. Custom extensions

Hazard3 implements a small number of custom extensions. All are optional: custom extensions are only included if the

relevant feature flags are set to 1 when instantiating the processor (Section 3.8.8). Hazard3 is always a conforming

RISC-V implementation; when these extensions are disabled, it is also a standard RISC-V implementation.

3.8. Hazard3 processor
286

RP2350 Datasheet

If any one of these extensions is enabled, the x bit in MISA is set to indicate the presence of a non-standard extension.

3.8.6.1. Xh3irq: Hazard3 interrupt controller

Xh3irq controls up to 512 external interrupts, with up to 16 levels of pre-emption. It is architected as a layer on top of the

standard mip.meip external interrupt line, and all standard RISC-V interrupt behaviour still applies. This extension adds no

new instructions, but does add several CSRs:

• MEIEA: external interrupt enable array
• MEIPA: external interrupt pending array
• MEIFA: external interrupt force array
• MEIPRA: external interrupt priority array
• MEINEXT: get next external interrupt
• MEICONTEXT: external interrupt context register

Xh3irq is geared towards supporting interrupt handlers as bare C functions, with dispatch implemented in software and

pre-emption priority logic implemented in hardware. However, the exact interrupt ABI is up to the implementation of the

soft dispatch routine installed as the mip.meip external interrupt handler.

3.8.6.1.1. Array CSRs

RISC-V CSRs are ideal for interrupt controls because they are closely coupled to the processor, offer native atomic

set/clear accesses, and can be accessed in a single instruction without first having to materialise an address. However

there are issues with using CSRs for large bit arrays, such as interrupt enables:

• The CSR address space is limited
• CSRs can not be addressed indirectly, so are difficult to iterate over
• Using a CSR to index other CSRs is problematic for interrupt handlers due to additional mutable state

Xh3irq uses the array CSR idiom to expose a large bit vector at a single CSR address, such as MEIEA. The upper half of

the CSR is a 16-bit window into the array, and the window is indexed by the LSBs of the write data for the same CSR

instruction.

For example, the following assembly code writes 0xa5a5 to bits 47:32 of the interrupt enable array, since the window

index is 0x2 and the window is 16 bits in size:

```c
    li a0, 0xa5a50002
    csrw RVCSR_MEIEA_OFFSET, a0
```

The following reads bits 63:48 of the interrupt pending array into register a0, since the index is 0x3, and a CSR set of

0x0000 does not modify the window contents:

```c
    csrrsi a0, RVCSR_MEIPA_OFFSET, 0x3
```

Setting an arbitrary IRQ enable from C works as follows:

```c
void enable_irq(uint irq) {
    uint index = irq / 16;
    uint32_t mask = 1u << (irq % 16);
    asm (
```

3.8. Hazard3 processor
287

RP2350 Datasheet

```c
        "csrs 0xbe0, %0\n"
        : : "r" (index | (mask << 16))
    );
}
```

Getting an arbitrary IRQ pending flag from C is as follows:

```c
bool check_irq_pending(uint irq) {
    uint index = irq / 16;
    uint32_t csr_rdata;
    asm (
        "csrrs %0, 0xbe1, %1\n"
        : "=r" (csr_rdata)
        : "r" (index)
    );
    csr_rdata >>= 16;
    return csr_rdata & (1u << (irq % 16));
}
```

The SDK implements similar operations in the hardware_irq API.

Hazard3 supports up to 512 interrupts, which is one 16-bit window for each of the possible values of a 5-bit CSR

immediate.

3.8.6.1.2. Enable, pending, and force arrays

The MEIEA, MEIPA and MEIFA CSRs expose the interrupt enable, pending and force arrays respectively. Each array

contains one bit per system-level interrupt line, of which there are 52 lines in total. (See Section 3.2 for the assignment

of system IRQ numbers to peripherals.)

The interrupt enable array gates the entry of interrupt signals into the core. When a bit is clear in MEIEA, the

corresponding interrupt signal is ignored. When a bit is set, assertion of the corresponding interrupt signal will send the

core to the meip vector as soon as it is safe and appropriate to do so. From there, the meip handler vectors to the correct

handler, after saving the interruptee’s context.

The SDK irq_set_enabled() function in the hardware_irq library is a convenient way to manipulate the interrupt enable

array.

The interrupt pending array displays the current status of the system-level interrupt signals. Interrupts are visible in

MEIPA even if the corresponding bit is clear in MEIEA, and even if the interrupt has insufficient priority to interrupt the

core at this time. This register is read-only: bits in MEIPA clear automatically when the corresponding interrupt source

de-asserts. For example, a UART RX FIFO interrupt should clear on its own after data has been read from the FIFO.

The interrupt force array causes interrupts to appear pending, even when the corresponding system-level interrupt

signal is de-asserted. When a bit is set in MEIFA, the corresponding bit in MEIPA reads as 1, and will interrupt the core if

it meets the usual prerequisites.

MEIFA bits clear automatically when the corresponding interrupt is sampled from MEINEXT. It is not necessary to write

a 1 bit to MEINEXT.UPDATE for the interrupt force bit to clear. This means setting an MEIFA bit should cause the

interrupt to be taken once. Normal csrw and csrc instructions will also clear MEIFA.

Six spare interrupt lines 46 through 51, referred to as SPAREIRQ_IRQ_0 through SPAREIRQ_IRQ_5 in the SDK, deliberately do not

connect to system-level hardware. However they are still fully implemented in the interrupt controller, and fire when set

pending in MEIFA. For example, a fast interrupt top-half handler can schedule its longer-running bottom half to run at a

lower priority, or a high-priority context switch interrupt might schedule a context switch to take place at a lower priority

in order to clear interrupt frames off the stack.

3.8. Hazard3 processor
288

RP2350 Datasheet

3.8.6.1.3. Next interrupt register

MEINEXT always displays the next interrupt that should be handled, taking priority order into account. Interrupts appear

in MEINEXT when they meet all of the following criteria:

1. Pending in MEIPA

2. Enabled in MEIEA

3. Of priority greater than or equal to MEICONTEXT.PPREEMPT

The value returned is the IRQ number of the highest-priority interrupt that meets these three criteria, left-shifted by two.

When multiple interrupts have the highest priority, the lowest-numbered of those interrupts is chosen, as a tie-break.

The MSB of MEINEXT is set to indicate there were no eligible interrupts, and the remaining bits are undefined in this

case. Software should repeatedly read MEINEXT until all available interrupts are exhausted. The bltz and bgez

instructions are a convenient way to test the MSB of a register.

The purpose of rule 3 above is to ensure that any interrupt that may already be in progress in a pre-empted interrupt

frame is not re-entered in the current frame. Without this rule, multiple executions of the same interrupt handler could be

interleaved due to pre-emption by other handlers. Programmers are usually surprised when this happens.

MEINEXT.UPDATE is a write-only field which instructs hardware to update MEICONTEXT with information about the

interrupt displayed in MEINEXT on that cycle. Section 3.8.6.1.5 goes into more detail about context register updates.

IMPORTANT

MEINEXT is constantly changing as interrupt signals come and go. The write to MEINEXT.UPDATE must be the

same instruction that reads the interrupt index from MEINEXT to avoid a data race. This can be achieved with a csrrw

or csrrwi instruction.

3.8.6.1.4. Interrupt priority

The interrupt priority array MEIPRA implements a four-bit field per interrupt. In hardware, numerically higher (unsigned)

MEIPRA values have higher priority, taking precedence over lower-priority interrupts. The irq_set_priority() SDK

function uses the opposite convention, with lower numeric values indicating greater precedence. This section uses the

hardware numbering.

The interrupt priority in MEIPRA determines three things:

1. Whether the interrupt source is permitted to interrupt the core at this moment: must be greater than or equal

MEICONTEXT.PREEMPT

2. Whether the interrupt source can appear in MEINEXT: must be greater than or equal to MEICONTEXT.PPREEMPT

3. What order this interrupt will appear in when there are multiple candidates for MEINEXT

When MEICONTEXT is correctly saved and restored, PREEMPT and PPREEMPT are both zero outside of interrupt

handlers, and PREEMPT is strictly greater than PPREEMPT when inside an interrupt handler. Together they define the

band of interrupt priorities which may be processed without any pushing or popping of interrupt stack frames.

Manipulating interrupt priority outside of interrupts is safe. There is no need to disable interrupts when writing to the

priority array. Manipulating interrupt priority inside of an interrupt handler requires care: hardware operation is well-

defined, but the results can be surprising. Be wary of the following cases:

1. Increasing the priority of the current handler: if still enabled and pending, you will instantly pre-empt yourself.

2. Increasing the priority of a different interrupt, with priority lower than MEICONTEXT.PPREEMPT: this interrupt may

already be in progress in a frame that was pre-empted in order to run your handler. Increasing the priority may

cause it to execute in a higher frame before returning to the original frame where it is still in progress, thereby

interleaving with its own execution.

PPREEMPT is guaranteed to be no greater than the current handler priority if MEICONTEXT is correctly saved/restored,

since it contains the previous value of PREEMPT at the time a pre-emption took place, and interrupts lower than

3.8. Hazard3 processor
289

RP2350 Datasheet

PREEMPT can not interrupt the core. Therefore a safe approximation for case 2 above is: do not increase (by any

amount) the priority of a handler with lower priority than the currently running handler.

If an interrupt must increase the priority of a lower-priority interrupt, one solution is to queue up interrupt priority

updates, and pend a lowest-priority handler assigned to one of the spare IRQs, which processes the enqueued updates.

You can pend this handler manually by setting its bit in MEIFA. The handler will run last thing before returning to

foreground code. This is safe because an interrupt of the lowest priority by definition can not have pre-empted any other

interrupts.

3.8.6.1.5. Interrupt context management

The MEICONTEXT register has two functions: manage the core pre-emption priority across multiple pre-empting

interrupt stack frames, and help software track which interrupt handler it is currently executing, if any.

MEICONTEXT.PREEMPT, MEICONTEXT.PPREEMPT and MEICONTEXT.PPPREEMPT form a three-level stack of pre-

emption priorities:

• PREEMPT sets the minimum interrupt priority which interrupts the core
• PPREEMPT sets the minimum interrupt priority which appears in MEINEXT: this avoids redundant execution of

interrupt handlers which may have been pre-empted
• PPPREEMPT has no hardware function other than save/restore of PPREEMPT

When entering the MIP.MEIP vector, hardware atomically performs the following updates to MEICONTEXT

simultaneous to the standard trap entry sequence described in Section 3.8.4:

1. Save the current value of MEICONTEXT.PPREEMPT to PPPREEMPT

2. Save the current value of MEICONTEXT.PREEMPT to PPREEMPT

3. Write one plus the priority of the IRQ which caused this interrupt to MEICONTEXT.PREEMPT

4. Write 1 to MEICONTEXT.MRETEIRQ, to enable priority restore on next mret

The standard trap entry sequence includes clearing MSTATUS.MIE, so interrupts are disabled at the start of the handler.

To implement pre-emption, the MIP.MEIP handler must re-enable interrupts after its context save critical section. This

should include saving MEICONTEXT, MSTATUS, MEPC, and the caller-saved general-purpose registers.

Any trap entry not caused by MIP.MEIP clears MRETEIRQ. Trap exit (mret) also clears MRETEIRQ.

A trap exit where MEICONTEXT.MRETEIRQ is set atomically performs the following updates to MEICONTEXT

simultaneous to the standard trap exit sequence:

1. Restore MEICONTEXT.PREEMPT from MEICONTEXT.PPREEMPT

2. Restore MEICONTEXT.PPREEMPT from MEICONTEXT.PPPREEMPT

3. Write 0 to MEICONTEXT.PPPREEMPT

The MRETEIRQ flag allows hardware to match each MIP.MEIP vector entry with its associated mret. This balances

pushes and pops of the PREEMPT priority stack. When there is no pre-emption, and no exceptions raised within

interrupt handlers, MRETEIRQ can be left in place in the MEICONTEXT.MRETEIRQ register. Otherwise, you must save

MEICONTEXT upon entering the external interrupt vector and restore it before the mret at the end of the handler.

Interrupts must be disabled during save/restore.

Writing 1 to MEINEXT.UPDATE updates MEICONTEXT as follows:

1. Write MEINEXT.NOIRQ to MEICONTEXT.NOIRQ

2. Write MEINEXT.IRQ (the IRQ number) to MEICONTEXT.IRQ

3. If MEINEXT.NOIRQ is…

◦Clear: Write one plus the priority of MEINEXT.IRQ to MEICONTEXT.PREEMPT

3.8. Hazard3 processor
290

RP2350 Datasheet

◦Set: Write 0x10 to MEICONTEXT.PREEMPT (greater than any interrupt priority in MEIPRA)

MEICONTEXT.IRQ and NOIRQ help code determine in which interrupt handler it is running. MEICONTEXT should be

saved/restored by interrupts which pre-empt the current one, so is safe to check these fields during the handler.

The update to MEICONTEXT.PREEMPT upon writing MEINEXT.UPDATE ensures the core will be pre-empted by

interrupts higher-priority than the one it is about to enter. Equally important, it ensures the core is not pre-empted by

lower or equal priority interrupts, including the one whose handler it is about to enter.

To avoid awkward interactions between the MIP.MEIP handler, which should be aware of the Xh3irq extension, and the

MTIP/MSIP handlers, which may not be, it’s best to avoid pre-emption of the former by the latter.

MEICONTEXT.CLEARTS, MTIESAVE and MSIESAVE support disabling and restoring the timer/software interrupt

enables as part of the MEICONTEXT CSR accesses that take place during context save/restore in the MEIP handler.

3.8.6.1.6. Minimal handler example

This example demonstrates a minimal meip handler which dispatches to an array of C-function interrupt handlers,

without enabling pre-emption. In this case the priorities configured in MEIPRA still determine the order in which

interrupts are entered when multiple are asserted, but when an interrupt handler starts running, no other interrupts are

serviced until that handler completes.

```c
#include "hardware/regs/rvcsr.h"
isr_riscv_machine_external_irq:
    // Save all caller saves and temporaries before entering a C ABI function.
    // Note mstatus.mie is cleared by hardware on interrupt entry, and
    // we're going to leave it clear.
    addi sp, sp, -64
    sw ra,  0(sp)
    sw t0,  4(sp)
    sw t1,  8(sp)
    sw t2, 12(sp)
    sw a0, 16(sp)
    sw a1, 20(sp)
    sw a2, 24(sp)
    sw a3, 28(sp)
    sw a4, 32(sp)
    sw a5, 36(sp)
    sw a6, 40(sp)
    sw a7, 44(sp)
    sw t3, 48(sp)
    sw t4, 52(sp)
    sw t5, 56(sp)
    sw t6, 60(sp)
get_first_irq:
    // Sample the current highest-priority active IRQ (left-shifted by 2) from
    // meinext. Don't set the `update` bit as we aren't saving/restoring meicontext --
    // this is fine, just means you can't check meicontext to see whether you are in an IRQ.
    csrr a0, RVCSR_MEINEXT_OFFSET
    // MSB will be set if there is no active IRQ at the current priority level
    bltz a0, no_more_irqs
dispatch_irq:
    // Load indexed table entry and jump through it. No bounds checking is necessary
    // because the hardware will not return a nonexistent IRQ.
    lui a1, %hi(__soft_vector_table)
    add a1, a1, a0
    lw a1, %lo(__soft_vector_table)(a1)
    jalr ra, a1
get_next_irq:
```

3.8. Hazard3 processor
291

RP2350 Datasheet

```c
    // Get the next-highest-priority IRQ
    csrr a0, RVCSR_MEINEXT_OFFSET
    // MSB will be set if there is no active IRQ at the current priority level
    bgez a0, dispatch_irq
no_more_irqs:
    // Restore saved context and return from IRQ
    lw ra,  0(sp)
    lw t0,  4(sp)
    lw t1,  8(sp)
    lw t2, 12(sp)
    lw a0, 16(sp)
    lw a1, 20(sp)
    lw a2, 24(sp)
    lw a3, 28(sp)
    lw a4, 32(sp)
    lw a5, 36(sp)
    lw a6, 40(sp)
    lw a7, 44(sp)
    lw t3, 48(sp)
    lw t4, 52(sp)
    lw t5, 56(sp)
    lw t6, 60(sp)
    addi sp, sp, 64
    mret
// Array of function pointers for interrupt handlers
.section ".bss"
.p2align 2
.global __soft_vector_table
__soft_vector_table:
.space 52 * 4
```

Since the handler loops on meinext until no more interrupts are pending, multiple interrupts are processed with a single

save/restore of the caller saves and temporaries.

The pending status of each IRQ in MEIPA clears when the corresponding peripheral de-asserts its interrupt output. A

correctly programmed interrupt handler should cause the peripheral interrupt to de-assert, so each successive read

from meinext will return a new interrupt. Because meinext always returns the highest-priority active interrupt, this loop

iterates over active interrupts in descending priority order.

The overhead of performing the register save/restore in software is minimal because the save/restore routine is limited

by bus bandwidth, not by instruction execution overhead. This also makes the hardware more flexible because the same

hardware can support multiple interrupt ABIs.

3.8.6.2. Xh3pmpm: M-mode PMP regions

This extension adds a new M-mode CSR, PMPCFGM0, which allows a PMP region to be enforced in M-mode without

locking the region.

This is useful when the PMP is used for non-security-related purposes such as stack guarding, or trapping and

emulation of peripheral accesses.

3.8.6.3. Xh3power: Hazard3 power management

This extension adds a new M-mode CSR (MSLEEP), and two new hint instructions, h3.block and h3.unblock, in the slt

nop-compatible custom hint space.

The msleep CSR controls how deeply the processor sleeps in the WFI sleep state. By default, a WFI is implemented as a

3.8. Hazard3 processor
292

RP2350 Datasheet

normal pipeline stall. By configuring msleep appropriately, the processor can gate its own clock when asleep or, with a

simple 4-phase req/ack handshake, negotiate power up/down of external hardware with an external power controller.

These options can improve the sleep current at the cost of greater wakeup latency.

The hints allow processors to sleep until woken by other processors in a multiprocessor environment. They are

implemented on top of the standard WFI state, which means they interact in the same way with external debug, and

benefit from the same deep sleep states in msleep.

3.8.6.3.1. h3.block

Enter a WFI sleep state until either an unblock signal is received, or an interrupt is asserted that would cause a WFI to

exit.

If mstatus.tw is set, attempting to execute this instruction in privilege modes lower than M-mode will generate an illegal

instruction exception.

If an unblock signal has been received in the time since the last h3.block, this instruction executes as a nop, and the

processor does not enter the sleep state. Conceptually, the sleep state falls through immediately because the

corresponding unblock signal has already been received.

An unblock signal is received when a neighbouring processor (the exact definition of "neighbouring" being left to the

implementer) executes an h3.unblock instruction, or for some other platform-defined reason.

This instruction is encoded as slt x0, x0, x0, which is part of the custom nop-compatible hint encoding space.

Example C macro:

```c
#define __h3_block() asm ("slt x0, x0, x0")
```

Example assembly macro:

```c
.macro h3.block
    slt x0, x0, x0
.endm
```

3.8.6.3.2. h3.unblock

Post an unblock signal to other processors in the system. For example, to notify another processor that a work queue is

now non-empty.

If mstatus.tw is set, attempting to execute this instruction in privilege modes lower than M-mode will generate an illegal

instruction exception.

This instruction is encoded as slt x0, x0, x1, which is part of the custom nop-compatible hint encoding space.

Example C macro:

```c
#define __h3_unblock() asm ("slt x0, x0, x1")
```

Example assembly macro:

3.8. Hazard3 processor
293

RP2350 Datasheet

```c
.macro h3.unblock
    slt x0, x0, x1
.endm
```

3.8.6.4. Xh3bextm: Hazard3 bit extract multiple

This is a small extension with multi-bit versions of the "bit extract" instructions from Zbs, used for extracting small,

contiguous bit fields.

3.8.6.4.1. h3.bextm

"Bit extract multiple", a multi-bit version of the bext instruction from Zbs. Perform a right-shift followed by a mask of 1-8

LSBs.

Encoding (R-type):

| Bits | Name | Value | Description |
| --- | --- | --- | --- |
| 31:29 | funct7[6:4] | 0b000 | RES0 |
| 28:26 | size | - | Number of ones in mask, values 0→7 encode 1→8 bits. |
| 25 | funct7[0] | 0b0 | RES0, because aligns with shamt[5] of potential RV64 version of h3.bextmi |
| 24:20 | rs2 | - | Source register 2 (shift amount) |
| 19:15 | rs1 | - | Source register 1 |
| 14:12 | funct3 | 0b000 | h3.bextm |
| 11:7 | rd | - | Destination register |
| 6:2 | opc | 0b01011 | custom0 opcode |
| 1:0 | size | 0b11 | 32-bit instruction |

Example C macro (using GCC statement expressions):

```c
// nbits must be a constant expression
#define __h3_bextm(nbits, rs1, rs2) ({\
    uint32_t __h3_bextm_rd; \
    asm (".insn r 0x0b, 0, %3, %0, %1, %2"\
        : "=r" (__h3_bextm_rd) \
        : "r" (rs1), "r" (rs2), "i" ((((nbits) - 1) & 0x7) << 1)\
    ); \
    __h3_bextm_rd; \
})
```

Example assembly macro:

```c
// rd = (rs1 >> rs2[4:0]) & ~(-1 << nbits)
.macro h3.bextm rd rs1 rs2 nbits
.if (\nbits < 1) || (\nbits > 8)
.err
.endif
#if NO_HAZARD3_CUSTOM
```

3.8. Hazard3 processor
294

RP2350 Datasheet

```c
    srl  \rd, \rs1, \rs2
    andi \rd, \rd, ((1 << \nbits) - 1)
#else
.insn r 0x0b, 0x0, (((\nbits - 1) & 0x7 ) << 1), \rd, \rs1, \rs2
#endif
.endm
```

3.8.6.4.2. h3.bextmi

Immediate variant of h3.bextm.

Encoding (I-type):

| Bits | Name | Value | Description |
| --- | --- | --- | --- |
| 31:29 | imm[11:9] | 0b000 | RES0 |
| 28:26 | size | - | Number of ones in mask, values 0→7 encode 1→8 bits. |
| 25 | imm[5] | 0b0 | RES0, for potential future RV64 version |
| 24:20 | shamt | - | Shift amount, 0 through 31 |
| 19:15 | rs1 | - | Source register 1 |
| 14:12 | funct3 | 0b100 | h3.bextmi |
| 11:7 | rd | - | Destination register |
| 6:2 | opc | 0b01011 | custom0 opcode |
| 1:0 | size | 0b11 | 32-bit instruction |

Example C macro (using GCC statement expressions):

```c
// nbits and shamt must be constant expressions
#define __h3_bextmi(nbits, rs1, shamt) ({\
    uint32_t __h3_bextmi_rd; \
    asm (".insn i 0x0b, 0x4, %0, %1, %2"\
        : "=r" (__h3_bextmi_rd) \
        : "r" (rs1), "i" ((((nbits) - 1) & 0x7) << 6 | ((shamt) & 0x1f)) \
    ); \
    __h3_bextmi_rd; \
})
```

Example assembly macro:

```c
// rd = (rs1 >> shamt) & ~(-1 << nbits)
.macro h3.bextmi rd rs1 shamt nbits
.if (\nbits < 1) || (\nbits > 8)
.err
.endif
.if (\shamt < 0) || (\shamt > 31)
.err
.endif
#if NO_HAZARD3_CUSTOM
    srli \rd, \rs1, \shamt
    andi \rd, \rd, ((1 << \nbits) - 1)
#else
.insn i 0x0b, 0x4, \rd, \rs1, (\shamt & 0x1f) | (((\nbits - 1) & 0x7 ) << 6)
```

3.8. Hazard3 processor
295

RP2350 Datasheet

```c
#endif
.endm
```

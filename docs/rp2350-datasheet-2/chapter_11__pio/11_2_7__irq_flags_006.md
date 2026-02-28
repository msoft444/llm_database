---
source_pdf: rp2350-datasheet-2.pdf
repository: llm_database
chapter: Chapter 11. PIO
section: 11.2.7. IRQ flags
pages: 885-885
type: technical_spec
generated_at: 2026-02-28T17:43:10.557178+00:00
---

# 11.2.7. IRQ flags

RP2350 Datasheet

FIFOs also generate data request (DREQ) signals, which allow a system DMA controller to pace its reads/writes based

on the presence of data in an RX FIFO, or space for new data in a TX FIFO. This allows a processor to set up a long

transaction, potentially involving many kilobytes of data, which will proceed with no further processor intervention.

Often, a state machine only transfers data in one direction. In this case, the SHIFTCTRL_FJOIN option can merge the two

FIFOs into a single 8-entry FIFO that only goes in one direction. This is useful for high-bandwidth interfaces such as DPI.

11.2.5. Stalling

State machines may momentarily pause execution for a number of reasons:

• A WAIT instruction’s condition is not yet met
• A blocking PULL when the TX FIFO is empty, or a blocking PUSH when the RX FIFO is full
• An IRQ WAIT instruction which has set an IRQ flag, and is waiting for it to clear
• An OUT instruction when autopull is enabled, and OSR has already reached its shift threshold
• An IN instruction when autopush is enabled, ISR reaches its shift threshold, and the RX FIFO is full

In this case, the program counter does not advance, and the state machine will continue executing this instruction on

the next cycle. If the instruction specifies some number of delay cycles before the next instruction starts, these do not

begin until after the stall clears.

NOTE

Side-set (Section 11.5.1) is not affected by stalls, and always takes place on the first cycle of the attached

instruction.

11.2.6. Pin mapping

PIO controls the output level and direction of up to 32 GPIOs, and can observe their input levels. On every system clock

cycle, each state machine may do none, one, or both of the following:

• Change the level or direction of some GPIOs via an OUT or SET instruction, or read some GPIOs via an IN instruction
• Change the level or direction of some GPIOs via a side-set operation

Each of these operations uses one of four contiguous ranges of GPIOs, with the base and count of each range

configured via each state machine’s PINCTRL register. There is a range for each of OUT, SET, IN and side-set operations.

Each range can cover any of the GPIOs accessible to a given PIO block (on RP2350 this is the 30 user GPIOs), and the

ranges can overlap.

For each individual GPIO output (level and direction separately), PIO considers all 8 writes that may have occurred on

that cycle, and applies the write from the highest-numbered state machine. If the same state machine performs a SET

/OUT and a side-set on the same GPIO simultaneously, the side-set is used. If no state machine writes to this GPIO

output, its value does not change from the previous cycle.

Generally each state machine’s outputs are mapped to a distinct group of GPIOs, implementing some peripheral

interface.

11.2.7. IRQ flags

IRQ flags are state bits which can be set or cleared by state machines or the system. There are 8 in total: all 8 are visible

to all state machines, and the lower 4 can also be masked into one of PIO’s interrupt request lines, via the IRQ0_INTE and

IRQ1_INTE control registers.

They have two main uses:

11.2. Programmer’s model
884

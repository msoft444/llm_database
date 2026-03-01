---
source_pdf: rp2350-datasheet-4.pdf
repository: llm_database
chapter: Chapter 11. PIO
section: 11.2.5. Stalling
pages: 885-885
type: technical_spec
generated_at: 2026-03-01T03:27:49.792955+00:00
---

# 11.2.5. Stalling

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

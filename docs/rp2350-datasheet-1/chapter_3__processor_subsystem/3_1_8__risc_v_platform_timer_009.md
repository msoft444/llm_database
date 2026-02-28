---
source_pdf: rp2350-datasheet-1.pdf
repository: llm_database
chapter: Chapter 3. Processor subsystem
section: 3.1.8. RISC-V platform timer
pages: 44-44
type: technical_spec
generated_at: 2026-02-28T17:22:56.644477+00:00
---

# 3.1.8. RISC-V platform timer

Whereas the mailbox FIFOs are used for cross-core events whose count and order is important, doorbells are used for
events which are accumulative (i.e. may post multiple times, but only answered once) and which can be responded to in
any order.
Writing a non-zero value to the DOORBELL_OUT_SET register raises the opposite core’s doorbell interrupt. The interrupt
remains raised until all bits are cleared. Generally, the opposite core enters its doorbell interrupt handler, reads its
DOORBELL_IN_CLR register to get the mask of active doorbell flags, and then writes back to acknowledge and clear the
interrupt.
The DOORBELL_IN_SET register allows a processor to ring its own doorbell. This is useful when the routine which rings
a doorbell can be scheduled on either core. Likewise, for symmetry, a processor can clear the opposite core’s doorbell
flags using the DOORBELL_OUT_CLR register: this is useful for setup code, but should be avoided in general because of
the potential for race conditions when acknowledging interrupts meant for the opposite core.
At any time, a core can read back its DOORBELL_OUT_SET or DOORBELL_OUT_CLR register (they return the same
result) to see the status of doorbell interrupts posted to the opposite core. Likewise, reading either DOORBELL_IN_SET
or DOORBELL_IN_CLR returns the status of doorbell interrupts posted to this core.
NOTE
RP2350 has separate per-core doorbell interrupt signals and doorbell registers for Secure and Non-secure SIO
banks. Non-secure doorbells are posted on SIO_IRQ_BELL_NS, interrupt number 28. See Section 3.1.1.
3.1.7. Integer divider
RP2040’s memory-mapped integer divider peripheral is not present on RP2350, since the processors support divide
instructions. The address space previously allocated for the divider registers is now reserved.
3.1.8. RISC-V platform timer
This 64-bit timer is a standard peripheral described in the RISC-V privileged specification, usable equally by the Arm and
RISC-V processors on RP2350. It drives the per-core SIO_IRQ_MTIMECMP system-level interrupt (Section 3.2), as well as the
mip.mtip timer interrupt on the RISC-V processors.
There is a single 64-bit counter, shared between both cores. The low and high half can be accessed through the MTIME
and MTIMEH SIO registers. Use the following procedure to safely read the 64-bit time using 32-bit register accesses:
1. Read the upper half, MTIMEH.
2. Read the lower half, MTIME.
3. Read the upper half again.
4. Loop if the two upper-half reads returned different values.
This is similar to the procedure for reading RP2350 system timers (Section 12.8). The loop should only happen once,
when the timer is read at exactly the instant of a 32-bit rollover, and even this is only occasional. If you require constant-
time operation, you can instead zero the lower half when the two upper-half reads differ.
Timer interrupts are generated based on a per-core 64-bit time comparison value, accessed through the MTIMECMP
and MTIMECMPH SIO registers. Each core gets its own copy of these registers, accessed at the same address. The per-
core interrupt is asserted whenever the current time indicated in the MTIME registers is greater than or equal to that
core’s MTIMECMP. Use the following sequence to write a new 64-bit timer comparison value without causing spurious
interrupts:
1. Write all-ones to MTIMECMP (guaranteed greater than or equal to the old value, and the lower half of the target
value).
2. Write the upper half of the target value to MTIMECMPH (combined 64-bit value is still greater than or equal to the
target value).
RP2350 Datasheet
3.1. SIO
43


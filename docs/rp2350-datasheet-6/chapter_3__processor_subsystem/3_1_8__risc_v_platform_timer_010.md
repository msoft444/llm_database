---
source_pdf: rp2350-datasheet-6.pdf
repository: llm_database
chapter: Chapter 3. Processor subsystem
section: 3.1.8. RISC-V platform timer
pages: 44-45
type: technical_spec
generated_at: 2026-03-01T05:39:21.783275+00:00
---

# 3.1.8. RISC-V platform timer

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

3.1. SIO
43

RP2350 Datasheet

3. Write the lower half of the target value to MTIMECMP.

The RISC-V timer can count either ticks from the system-level tick generator (Section 8.5), or system clock cycles,

selected by the MTIME_CTRL register. Use a 1 microsecond time base for compatibility with most RISC-V software.

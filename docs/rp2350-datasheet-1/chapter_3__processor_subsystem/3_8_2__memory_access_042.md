---
source_pdf: rp2350-datasheet-1.pdf
repository: llm_database
chapter: Chapter 3. Processor subsystem
section: 3.8.2. Memory access
pages: 279-279
type: technical_spec
generated_at: 2026-02-28T17:22:56.644477+00:00
---

# 3.8.2. Memory access

Operation: execute the trap return sequence described in Section 3.8.4.
Privilege requirements: M-mode only.
wfi
Wait for interrupt.
Usage:
wfi
Operation: pause execution until the processor is interrupted, or enters Debug mode.
Privilege requirements: M-mode is always permitted. U-mode is permitted if MSTATUS.TW is clear.
wfi ignores the global interrupt enable, MSTATUS.MIE. It respects all other interrupt controls. For example:
• If MIP.MEIP is 1, MIE.MEIE is 1, and MSTATUS.MIE is 0, a wfi instruction falls through immediately without
pausing.
• In this example, setting MSTATUS.MIE to 1 would cause the core to immediately take the interrupt.
• If no bit is set in both MIP and MIE, the wfi stalls until there is at least one such bit.
When a wfi is interrupted, the exception return address MEPC points to the instruction following the wfi.
When the debugger halts the core during a wfi, DPC points to the instruction immediately following the wfi
instruction. wfi executes as a no-op under instruction single-stepping (it does not stall), and under Debug-mode
execution in the Program Buffer.
Hazard3’s MSLEEP CSR controls additional power-saving measures the core can implement during a wfi sleep
state.
3.8.2. Memory access
Hazard3 accesses memory within a 4 GB (232 bytes) physical address space. There is no address translation. Each
possible value of an integer register uniquely identifies a single byte in the physical address space. Multi-byte values
occupy consecutive byte addresses.
3.8.2.1. Endianness
Hazard3 is always little-endian for all load and store accesses. RISC-V instruction fetch is always little-endian.
This means in a multi-byte access such as a sw instruction (four bytes are transferred), data stored at higher byte
addresses has greater numerical significance. For example:
li a0, 0x0d0c0b0a            // materialise constant in register
la a4, some_global_variable  // materialise address (assume addr % 4 == 0)
sw a0, (a4)                  // 4-byte write to memory
lbu a0, 0(a4)                // load byte from addr + 0: 0x0a
lbu a1, 1(a4)                // load byte from addr + 1: 0x0b
lbu a2, 2(a4)                // load byte from addr + 2: 0x0c
lbu a3, 3(a4)                // load byte from addr + 3: 0x0d
RP2350 Datasheet
3.8. Hazard3 processor
278


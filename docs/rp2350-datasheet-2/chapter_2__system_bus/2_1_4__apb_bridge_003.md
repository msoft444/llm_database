---
source_pdf: rp2350-datasheet-2.pdf
repository: llm_database
chapter: Chapter 2. System bus
section: 2.1.4. APB bridge
pages: 27-27
type: technical_spec
generated_at: 2026-02-28T17:43:10.557178+00:00
---

# 2.1.4. APB bridge

RP2350 Datasheet

2.1.3. Atomic register access

Each peripheral register block is allocated 4 kB of address space, with registers accessed using one of 4 methods,

selected by address decode.

• Addr + 0x0000 : normal read write access
• Addr + 0x1000 : atomic XOR on write
• Addr + 0x2000 : atomic bitmask set on write
• Addr + 0x3000 : atomic bitmask clear on write

This allows software to modify individual fields of a control register without performing a read-modify-write sequence.

Instead, the peripheral itself modifies its contents in-place. Without this capability, it is difficult to safely access IO

registers when an interrupt service routine is concurrent with code running in the foreground, or when the two

processors run code in parallel.

The four atomic access aliases occupy a total of 16 kB. Native atomic writes take the same number of clock cycles as

normal writes. Most peripherals on RP2350 provide this functionality natively, but some peripherals (I2C, UART, SPI and

SSI) add this functionality using a bus interposer. The bus interposer translates upstream atomic writes into

downstream read-modify-write sequences at the boundary of the peripheral, at the cost of additional clock cycles.

Atomic writes that use a bus interposer take two additional clock cycles compared to normal writes.

The following registers do not support atomic register access:

• SIO (Section 3.1), though some individual registers (for example, GPIO) have set, clear, and XOR aliases.
• Any register accessed through the self-hosted CoreSight window, including Arm Mem-APs and the RISC-V Debug

Module.
• Standard Arm control registers on the Cortex-M33 private peripheral bus (PPB), except for Raspberry Pi-specific

registers on the EPPB.
• OTP programming registers accessed through the SBPI bridge.

2.1.4. APB bridge

The APB bridge provides an interface between the high-speed main AHB5 interconnect and the lower-bandwidth

peripherals. Unlike the AHB5 fabric, which offers zero-wait-state accesses everywhere, APB accesses take a minimum

of three cycles for a read, and four cycles for a write.

As a result, the throughput of the APB portion of the bus fabric is lower than the AHB5 portion. However, there is more

than sufficient bandwidth to saturate the APB serial peripherals.

The following APB ports contain asynchronous bus crossings, which insert additional stall cycles on top of the typical

cost of a read or write in the APB bridge:

• ADC
• HSTX_CTRL
• OTP
• POWMAN

The APB bridge implements a fixed timeout for stalled downstream transfers. The downstream bus may stall

indefinitely, such as when accessing an asynchronous bus crossing when the destination clock is stopped, or deadlock

conditions when accessing system APB registers through Mem-APs in the self-hosted debug window (Section 3.5.6).

When an APB transfer exceeds 65,535 cycles the APB bridge abandons the transfer and returns a bus fault. This keeps

the system bus available so that software or the debugger can diagnose the reason for the overly long transfer.

2.1. Bus fabric
26

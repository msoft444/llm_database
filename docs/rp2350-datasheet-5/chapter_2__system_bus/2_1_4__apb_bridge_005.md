---
source_pdf: rp2350-datasheet-5.pdf
repository: llm_database
chapter: Chapter 2. System bus
section: 2.1.4. APB bridge
pages: 27-27
type: technical_spec
generated_at: 2026-03-01T04:13:03.328193+00:00
---

# 2.1.4. APB bridge

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

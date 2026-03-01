---
source_pdf: rp2350-datasheet-4.pdf
repository: llm_database
chapter: Chapter 3. Processor subsystem
section: 3.5.6. Self-hosted debug
pages: 88-88
type: technical_spec
generated_at: 2026-03-01T03:27:49.792955+00:00
---

# 3.5.6. Self-hosted debug

3.5.6. Self-hosted debug

All APs shown in Figure 10, except for the RP-AP, have direct memory-mapped access from the system bus. This is

known as self-hosted debug, because with care it allows running a debug host (i.e. a debugger) directly on-system. It

can also be used to access the trace hardware, which can be used for self-hosted trace using the trace DMA FIFO. By

default only Secure access is permitted, as the processor debug presents an opportunity for Non-secure code to

interfere with the Secure context and/or perform Secure bus accesses.

The self-hosted debug window starts at address 0x40140000 (CORESIGHT_PERIPH_BASE). The offsets of the APs within

this window are the same as the APs' addresses when accessed from the SW-DP.

Because of the blocking nature of the AHB-AP’s DRW register, and its interactions with the Cortex-M33’s arbitration of

AHB-AP accesses with load/stores, certain accesses have potential to cause bus lockup due to circular bus stall

dependencies. In particular, cores may not access their own AHB-APs through the self-hosted debug window, and AHB-

APs may not access AHB-APs through the self-hosted debug window — attempting to do so will immediately return a

bus fault. To reduce the opportunities for deadlock, a full APB crossbar is used to connect the SW-DP and the self-

hosted debug port to the APs, so that for example self-hosted use of the Arm trace hardware will not interfere with an

external debugger attaching via the AHB-APs.

3.5. Debug
87

---
source_pdf: rp2350-datasheet-4.pdf
repository: llm_database
chapter: Chapter 2. System bus
section: 2.1.1. Bus priority
pages: 26-26
type: technical_spec
generated_at: 2026-03-01T03:27:49.792955+00:00
---

# 2.1.1. Bus priority

2.1.1. Bus priority

The main AHB5 crossbar implements a two-level bus priority scheme. Priority levels are configured separately for core

0, core 1, DMA read and DMA write, using the BUS_PRIORITY register in the BUSCTRL register block.

When a downstream subordinate receives multiple simultaneous access requests, the port serves high-priority (priority

level 1) managers before serving any requests from low-priority (priority 0) managers. If all requests come from

managers with the same priority level, the port applies a round-robin tie break, granting access to each manager in turn.

NOTE

Priority arbitration only applies when multiple managers attempt to access the same subordinate on the same cycle.

When multiple managers access different subordinates, e.g. different SRAM banks, the requests proceed

simultaneously.

A subordinate with zero wait states can be accessed once per system clock cycle. When accessing a subordinate with

zero wait states (e.g. SRAM), high-priority managers never experience delays caused by accesses from low-priority

managers. This guarantees latency and throughput for real-time use cases. However, it also means that low-priority

managers may stall until there is a free cycle.

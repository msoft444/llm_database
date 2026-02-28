---
source_pdf: rp2350-datasheet-1.pdf
repository: llm_database
chapter: Chapter 2. System bus
section: 2.1.2. Bus security filtering
pages: 26-26
type: technical_spec
generated_at: 2026-02-28T17:22:56.644477+00:00
---

# 2.1.2. Bus security filtering

NOTE
Instruction fetch from peripherals is physically disconnected, to avoid this IDAU-Exempt region ever becoming both
Non-secure-writable and Secure-executable. This includes USB RAM, OTP and boot RAM. See Section 10.2.2.
The SIO block, which was connected to the Cortex-M0+ IOPORT on RP2040, provides two AHB ports, each dedicated to
load/store access from one core.
The six managers can access any six different crossbar ports simultaneously. So, at a system clock of 150 MHz, the
maximum sustained bus bandwidth is 3.6 GB/s.
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
2.1.2. Bus security filtering
Every point where the fabric connects to a downstream AHB or APB peripheral is interposed by a bus security filter,
which enforces the following access control lists as defined by the ACCESSCTRL registers (Section 10.6):
• A list of who can access the port: core 0, core 1, DMA, debugger
• A list of the security states from which the port can be accessed: the four combinations of Secure/Non-secure and
Privileged/Unprivileged.
Accesses that fail either check are prevented from accessing the downstream port, and return a bus error upstream.
There are three exceptions, which do not implement bus security filters because they implement their own security
filtering internally:
• The ACCESSCTRL block itself, which is always world-readable, but filters writes on security and privilege
• Boot RAM, which is hardwired to Secure access only
• The single-cycle IO subsystem (SIO), which is internally banked over Secure and Non-secure
The Cortex-M Private Peripheral Bus (PPB) registers also lack ACCESSCTRL permissions because they are internal to
the processors, not accessed through the system bus. The PPB registers are internally banked over Secure and Non-
secure.
RP2350 Datasheet
2.1. Bus fabric
25


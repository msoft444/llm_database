---
source_pdf: rp2350-datasheet-1.pdf
repository: llm_database
chapter: Chapter 3. Processor subsystem
section: 3.7.2. Configuration
pages: 125-128
type: technical_spec
generated_at: 2026-02-28T17:22:56.644477+00:00
---

# 3.7.2. Configuration

NOTE
Full details of the Arm Cortex-M33 processor can be found in the Technical Reference Manual.
3.7.1. Features
The Arm Cortex-M33 processor provides the following features and benefits:
• An in-order issue pipeline
• Thumb-2 technology; for more information, see the Armv8-M Architecture Reference Manual
• Little-endian data accesses
• A Nested Vectored Interrupt Controller (NVIC) closely integrated with the processor
• A Floating Point Unit (FPU) supporting single-precision arithmetic
• Support for exception-continuable instructions, such as LDM, LDMDB, STM, STMDB, PUSH, POP, VLDM, VSTM,
VPUSH, and VPOP
• A low-cost debug solution that provides the ability to implement:
◦Breakpoints
◦Watchpoints
◦Tracing
◦System profiling
◦Support for printf() style debugging through an Instrumentation Trace Macrocell (ITM)
• Support for the Embedded Trace Macrocell (ETM) instruction trace option; for more information, see the Arm
CoreSight ETM-M33 Technical Reference Manual
• A coprocessor interface for external hardware accelerators
• Low-power features including architectural clock gating, sleep mode, and a power-aware system with Wake-up
Interrupt Controller (WIC)
• A memory system that includes memory protection and security attribution
3.7.2. Configuration
Each Arm Cortex-M33 processor in RP2350 is configured with the following features:
• FPU: Single precision FPU
• DSP: DSP extension
• SECEXT: Security extensions
• CPIF: coprocessor interface
• MPU_NS: 8 non-secure MPU regions
• MPU_S: 8 secure MPU regions
• SAU: 8 SAU regions
• IRQ: 52 external interrupts
• IRQLVL: 4 exception priority bits
• DBGLVL: Full debug set: 4 watchpoint, 8 breakpoint comparators, debug monitor
RP2350 Datasheet
3.7. Cortex-M33 processor
124


• ITM: DWT and ITM trace
• ETM: ETM trace
• MTB: no MTB trace
• WIC: Wake up interrupt controller
• WICLINES: 55: All external interrupts and 3 internal events: NMI, RVEX, Debug
• CTI: Cross trigger interface
• RAR: reset all registers on power up
• UNCROSS_I_D: Modify internal address map
• SBIST: no SBIST features
• CDE modules not used
• CDERTLID: RTL ID for system with multi Cortex-M33: 16
Architectural clock gating allows the processor core to support SLEEP and DEEPSLEEP power states by disabling the
clock to parts of the processor core. Power gating is not supported.
Each Cortex-M33 core has its own interrupt controller that can individually mask out interrupt sources as required. The
same interrupts route to both Cortex-M33 cores.
The processor supports the following interfaces:
• Code AHB (C-AHB) interface
• System AHB (S-AHB) interface
• External PPB (EPPB) APB interface
• Debug AHB (D-AHB) interface
The processor implements the following optional interfaces:
• Arm TrustZone technology, using the Armv8-M Security Extension supporting Secure and Non-secure states
• Memory Protection Units (MPUs), which you can configure to protect regions of memory
• Floating-point arithmetic functionality with support for single precision arithmetic
• Support for ETM trace
3.7.2.1. Modifications by Raspberry Pi
3.7.2.1.1. UNCROSS_I_D
The original Cortex-M33 processor design routes the following operations to either the Code or System port:
• instruction fetch
• load/stores
• debugger accesses
Accesses below address 0x20000000 route to the Code port. All other accesses route to the System port.
This routing strategy makes contention possible on both the internal bus matrix and the main system AHB5 crossbar.
The Cortex-M33 Technical Reference Manual describes this strategy in detail.
In RP2350, Raspberry Pi modified the Cortex-M33 bus matrix to:
• route all instruction fetch operations to the Code port
RP2350 Datasheet
3.7. Cortex-M33 processor
125


• route all load/stores and debugger accesses to the System port
This eliminates internal conflicts and improves performance in certain software use cases, e.g. when allocating both
code and data from a single unified SRAM pool.
In Section 3.7.2, we refer to this feature as UNCROSS_I_D.
There are no other modifications to the Cortex-M33 processor.
NOTE
This datasheet may refer to the Cortex-M33 Code and System ports as the instruction and data ports respectively (I
and D), to reflect this modification to the core’s integrated bus matrix.
3.7.2.2. Interfaces
The processor has various external interfaces:
Code and System AHB interfaces
Harvard AHB bus architecture supporting exclusive transactions and security state.
System AHB interface
The System AHB (S-AHB) interface is used for any instruction fetch and data access to the memory-mapped SRAM,
Peripheral, External RAM and External device, or Vendor_SYS regions of the Armv8-M memory map.
Code AHB interface
The Code AHB (C-AHB) interface is used for any instruction fetch and data access to the Code region of the Armv8-
M memory map.
External Private Peripheral Bus
The External PPB (EPPB) APB interface enables access to CoreSight-compatible debug and trace components in a
system connected to the processor.
Secure attribution interface
The processor has an interface that connects to an external Implementation Defined Attribution Unit (IDAU), which
enables your system to set security attributes based on address.
ATB interfaces
The ATB interfaces output trace data for debugging. The ATB interfaces are compatible with the CoreSight
architecture. See the Arm CoreSight Architecture Specification v2.0 for more information. The instruction ATB
interface is used by the ETM, and the instrumentation ATB interface is used by the Instrumentation Trace Macrocell
(ITM).
Micro Trace Buffer interfaces
The Micro Trace Buffer (MTB) AHB slave interface and SRAM interface are for the CoreSight Micro Trace Buffer.
Coprocessor interface
The coprocessor interface is designed for closely coupled external accelerator hardware.
Debug AHB interface
The Debug AHB (D-AHB) slave interface allows a debugger access to registers, memory, and peripherals. The D-
AHB interface provides debug access to the processor and the complete memory map.
Cross Trigger Interface
The processor includes a Cross Trigger Interface (CTI) Unit that has an interface that is suitable for connection to
external CoreSight components using a Cross Trigger Matrix (CTM).
Power control interface
The processor supports a number of internal power domains that can be enabled and disabled using Q-channel
interfaces connected to a Power Management Unit (PMU) in the system.
RP2350 Datasheet
3.7. Cortex-M33 processor
126


3.7.2.3. Security attribution and memory protection
The Cortex-M33 processor supports the Armv8-M Protected Memory System Architecture (PMSA) that provides
programmable support for memory protection using a number of software controllable regions. RP2350 supports 8
programmable regions.
PMSA allows privileged software to assign access permissions to a memory region. When unprivileged software
attempts to access the region, a fault exception is triggered. PMSA includes fault status registers that allow an
exception handler to determine the source of the fault, apply corrective action, and notify the system. This reduces the
potential impact of incorrectly-written application code.
The Cortex-M33 processor also includes support for defining memory regions as Secure or Non-secure, as defined in
the Armv8-M Security Extension. This protects memory regions from accesses with an inappropriate level of security.
3.7.2.4. Floating-point unit (FPU)
The FPU provides:
• Instructions for single-precision (C programming language float type) data-processing operations
• Instructions for double-precision (C programming language double type) load and store operations
• Combined multiply-add instructions for increased precision (Fused MAC)
• Hardware support for conversion, addition, subtraction, multiplication, accumulate, division, and square-root
• Hardware support for denormals and all IEEE Standard 754-2008 rounding modes
• Thirty-two 32-bit single-precision registers or sixteen 64-bit double-precision registers
• Lazy floating-point context save
3.7.2.4.1. Lazy floating-point context save
This FPU function delays automated stacking of floating-point state until the ISR attempts to execute a floating-point
instruction. This reduces the latency to enter the ISR and removes floating-point context save for ISRs that do not use
floating-point.
3.7.2.5. NVIC
The Nested Vectored Interrupt Controller NVIC prioritizes external interrupt signals. Software can set the priority of each
interrupt. The NVIC and the Cortex-M33 processor core are closely coupled, providing low latency interrupt processing
and efficient processing of late arriving interrupts.
NOTE
"Nested" refers to the fact that interrupts can themselves be interrupted, by higher-priority interrupts. "Vectored"
refers to the hardware dispatching each interrupt to a distinct handler routine specified by a vector table. For more
details about nesting and vectoring behaviour, see the Armv8-M Architecture Reference Manual.
All NVIC registers are only accessible using word transfers. Any attempt to read or write a halfword or byte individually
is unpredictable.
NVIC registers are always little-endian.
The Nested Vectored Interrupt Controller (NVIC) is closely integrated with the core to achieve low-latency interrupt
processing.
Functions of the NVIC include:
RP2350 Datasheet
3.7. Cortex-M33 processor
127


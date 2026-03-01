---
source_pdf: rp2350-datasheet-4.pdf
repository: llm_database
chapter: Chapter 3. Processor subsystem
section: Chapter 3. Processor subsystem
pages: 36-36
type: technical_spec
generated_at: 2026-03-01T03:27:49.792955+00:00
---

# Chapter 3. Processor subsystem

RP2350 Datasheet

Chapter 3. Processor subsystem

![Page 36 figure](images/fig_p0036.png)

Figure 6. The RP2350

processor subsystem

connects two

processors to the

system bus, peripheral

interrupts, GPIOs, and

a Serial Wire Debug

(SWD) connection

from an external

debug host. It also

contains closely-

coupled peripherals,

and peripherals used

for synchronisation

and communication,

which are collectively

referred to as the

single-cycle IO

subsystem (SIO).

Arm
Cortex-M33
I
D

RISC-V
Hazard3
I
D

Arm
Cortex-M33
D
I

RISC-V
Hazard3
D
I

RP2350 is a symmetric dual-core system. Two cores operate simultaneously and independently, offering high

processing throughput and the ability to route interrupts to different cores to improve throughput and latency of

interrupt handling. The two cores have a symmetric view of the system bus; all memory resources on RP2350 are

accessible equally on both cores, with the same performance.

Each core has a pair of 32-bit AHB5 links to the system bus. One is used exclusively for instruction fetch, the other

exclusively for load or store instructions and debugger access. Each core can perform one instruction fetch and one

load or store access per cycle, provided there are no conflicts on the downstream bus ports.

There are two sockets for cores to attach to the system bus, referred to as core 0 and core 1 throughout this datasheet.

(They may synonymously be referred to as core0, core1, proc0 and proc1 in register documentation.) The processor

plugged into each socket is selectable at boot time:

• A Cortex-M33 processor, implementing the Armv8-M Main instruction set, plus extensions
• A Hazard3 processor, implementing the RV32IMAC instruction set, plus extensions

Cortex-M33 is the default option. Whichever processor is unused is held in reset with its clock gated at the top level.

Unused processors use zero dynamic power. See Section 3.9 for information about the architecture selection hardware.

The two Cortex-M33 instances are identical. They are configured with the Security, DSP and FPU extensions, as well as

8× SAU regions, 8× Secure MPU regions and 8× Non-secure MPU regions. Section 3.7 documents the Cortex-M33

processor as well as the specific configuration used on RP2350. The two Hazard3 instances are also identical to one

another; see Section 3.8 for the features and operation of the Hazard3 processors.

Chapter 3. Processor subsystem
35

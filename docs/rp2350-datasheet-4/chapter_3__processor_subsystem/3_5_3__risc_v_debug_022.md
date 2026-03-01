---
source_pdf: rp2350-datasheet-4.pdf
repository: llm_database
chapter: Chapter 3. Processor subsystem
section: 3.5.3. RISC-V debug
pages: 87-87
type: technical_spec
generated_at: 2026-03-01T03:27:49.792955+00:00
---

# 3.5.3. RISC-V debug

3.5.3. RISC-V debug

There is a single APB Mem-AP, at offset 0x0a000 in the debug address space, which provides access only to the RISC-V

Debug Module (DM). The DM is a standard component which the debugger uses to enumerate RISC-V harts present in

the system, debug software running on each hart, and access the system bus. It is defined in the RISC-V debug

specification, of which RP2350 implements version 0.13.2.

From the point of view of the RISC-V debug specification, the SW-DP and APB Mem-AP function jointly as the Debug

Transport Module for this system. The DM is located at offset 0x0 in the APB-AP’s downstream address space, and the

registers are word-sized and byte-addressed, meaning the DM register addresses in the debug specification must be

multiplied by 4 to get the correct APB address.

On RP2350, each core possesses exactly one hardware thread (hart). Core 0 has a hart ID of 0, and core 1 has a hart ID

of 1. These hart IDs match the hart index used in the DM. This DM is also equipped with the hart array mask select

extension, which allows multiple cores to be reset/halted/resumed simultaneously.

The DM is equipped with the System Bus Access (SBA) extension, which allows the debugger to access the system bus

without halting either core. This can be used for minimally intrusive debug techniques like Segger RTT. SBA accesses

3.5. Debug
86

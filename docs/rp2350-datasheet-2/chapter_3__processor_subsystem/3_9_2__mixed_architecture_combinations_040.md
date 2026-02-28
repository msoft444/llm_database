---
source_pdf: rp2350-datasheet-2.pdf
repository: llm_database
chapter: Chapter 3. Processor subsystem
section: 3.9.2. Mixed architecture combinations
pages: 337-337
type: technical_spec
generated_at: 2026-02-28T17:43:10.557178+00:00
---

# 3.9.2. Mixed architecture combinations

RP2350 Datasheet

As a result, the USB bootloader, which runs on both Arm and RISC-V, can accept a UF2 image download for either

architecture, and automatically boot it using the correct processors.

3.9.2. Mixed architecture combinations

The ARCHSEL register has one bit for each processor socket, so it is possible to request mixed combinations of Arm

and RISC-V processors: either Arm core 0 and RISC-V core 1, or RISC-V core 0 and Arm core 1.

Practical applications for this are limited, since this requires two separate program images. The two cores interoperate

normally, including shared exclusives via the global monitor: a shared variable can be safely, concurrently accessed by

an Arm processor performing ldrex, strex instructions and a RISC-V processor performing amoadd.w instructions, for

example.

Hardware supports debugging for a mixture of Arm and RISC-V processors, though this may prove challenging on the

host software side. Debug resources for unused processors are dynamically marked as non-PRESENT in the top-level

CoreSight ROM table.

3.9. Arm/RISC-V architecture switching
336

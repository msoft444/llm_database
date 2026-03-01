---
source_pdf: rp2350-datasheet-9.pdf
repository: llm_database
chapter: Chapter 3. Processor subsystem
section: 3.8.2. Memory access
pages: 279-280
type: technical_spec
generated_at: 2026-03-01T11:44:31.548839+00:00
---

# 3.8.2. Memory access

3.8.2. Memory access

Hazard3 accesses memory within a 4 GB (232 bytes) physical address space. There is no address translation. Each

possible value of an integer register uniquely identifies a single byte in the physical address space. Multi-byte values

occupy consecutive byte addresses.

3.8.2.1. Endianness

Hazard3 is always little-endian for all load and store accesses. RISC-V instruction fetch is always little-endian.

This means in a multi-byte access such as a sw instruction (four bytes are transferred), data stored at higher byte

addresses has greater numerical significance. For example:

```c
li a0, 0x0d0c0b0a            // materialise constant in register
la a4, some_global_variable  // materialise address (assume addr % 4 == 0)
sw a0, (a4)                  // 4-byte write to memory
lbu a0, 0(a4)                // load byte from addr + 0: 0x0a
lbu a1, 1(a4)                // load byte from addr + 1: 0x0b
lbu a2, 2(a4)                // load byte from addr + 2: 0x0c
lbu a3, 3(a4)                // load byte from addr + 3: 0x0d
```

3.8. Hazard3 processor
278

RP2350 Datasheet

3.8.2.2. Physical memory attributes

The RP2350 address space has the following physical memory attributes:

| Start | End | Description | Access | Atomicity | Idempotency |
| --- | --- | --- | --- | --- | --- |
| 0x00000000 | 0x00007fff | Boot ROM | No AMOs | RsrvNone, AMONone | Idempotent |
| 0x10000000 | 0x13ffffff | XIP, Cached | No AMOs | RsrvNone, AMONone | Idempotent |
| 0x14000000 | 0x17ffffff | XIP, Uncached | No AMOs | RsrvNone, AMONone | Idempotent |
| 0x18000000 | 0x1bffffff | XIP, Cache Maintenance | Write-only | RsrvNone, AMONone | Idempotent |
| 0x1c000000 | 0x1fffffff | XIP, Uncached + Untranslated | No AMOs | RsrvNone, AMONone | Idempotent |
| 0x20000000 | 0x20081fff | Main SRAM | Any | RsrvNonEventual, AMOArithmetic | Idempotent |
| 0x40000000 | 0x4fffffff | APB Peripherals | No AMOs, no instruction fetch | RsrvNone, AMONone | Non-idempotent |
| 0x50000000 | 0x5fffffff | AHB Peripherals | No AMOs, no instruction fetch | RsrvNone, AMONone | Non-idempotent |
| 0xd0000000 | 0xdfffffff | SIO Peripherals | No AMOs, no instruction fetch | RsrvNone, AMONone | Non-idempotent |

*Table 366. List of physical memory attributes for the RP2350 address space. Main SRAM supports all atomics, other addresses support none. Peripherals are non- idempotent, all other addresses are*

idempotent.

All addresses have Strong ordering. Any address not listed in Table 366 is a Vacant address. Accessing these

addresses has no effect other than returning a bus fault.

Hazard3’s PMP implementation requires that non-read-idempotent PMAs are also non-executable, because it enforces

execute permissions at the point an instruction is executed, rather than the point an instruction is fetched. Therefore all

non-idempotent locations in Table 366 are also non-executable. This is enforced at a lower level than the PMP, and

executing these addresses at any privilege level will always fault.

Cached XIP regions are not cacheable from a PMA point of view, because the cache is private to the memory controller.

Each system address is served by either a single cache controller or none, so coherence between harts is irrelevant. You

might have to perform manual cache maintenance following some operations like flash programming, but this is a

detail of the XIP subsystem, not the system-level memory model.

For definitions of these attributes, see section 3.6 of the RISC-V privileged ISA manual linked in Section 3.8.1.1.

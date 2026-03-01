---
source_pdf: rp2350-datasheet-8.pdf
repository: llm_database
chapter: Chapter 3. Processor subsystem
section: 3.6.1. GPIO coprocessor (GPIOC)
pages: 102-105
type: technical_spec
generated_at: 2026-03-01T08:45:43.944040+00:00
---

# 3.6.1. GPIO coprocessor (GPIOC)

3.6.1. GPIO coprocessor (GPIOC)

Coprocessor port 0 provides low-overhead access from the Cortex-M33 processors to the GPIO registers in the SIO

(Section 3.1.3). This enables a single coprocessor instruction to sample all 48 GPIOs, or to set/clear/write any single

GPIO, among other functionality.

Non-secure accesses are filtered according to the GPIO_NSMASK0 and GPIO_NSMASK1 registers in ACCESSCTRL.

GPIOs not granted for Non-secure use will ignore writes from the Non-secure state, and read back as zero when read

from the Non-secure state.

3.6.1.1. OUT mask write instructions

These instructions write to multiple bits in the SIO GPIO_OUT and GPIO_HI_OUT registers.

| Mnemonic | Armv8-M Instruction | Operation |
| --- | --- | --- |
| gpioc lo out put _ _ _ | mcr p0, #0, Rt, c0, c0 | sio hw→gpio out = Rt; _ _ |
| gpioc lo out xor _ _ _ | mcr p0, #1, Rt, c0, c0 | sio hw→gpio togl = Rt; _ _ |
| gpioc lo out set _ _ _ | mcr p0, #2, Rt, c0, c0 | sio hw→gpio set = Rt; _ _ |
| gpioc lo out clr _ _ _ | mcr p0, #3, Rt, c0, c0 | sio hw→gpio clr = Rt; _ _ |
| gpioc hi out put _ _ _ | mcr p0, #0, Rt, c0, c1 | sio hw→gpio hi out = Rt; _ _ _ |
| gpioc hi out xor _ _ _ | mcr p0, #1, Rt, c0, c1 | sio hw→gpio hi togl = Rt; _ _ _ |
| gpioc hi out set _ _ _ | mcr p0, #2, Rt, c0, c1 | sio hw→gpio hi set = Rt; _ _ _ |
| gpioc hi out clr _ _ _ | mcr p0, #3, Rt, c0, c1 | sio hw→gpio hi clr = Rt; _ _ _ |
| gpioc hilo out put _ _ _ | mcrr p0, #0, Rt, Rt2, c0 | Simultaneously: sio hw→gpio out = Rt; sio hw→gpio hi out = Rt2; _ _ _ _ _ |
| gpioc hilo out xor _ _ _ | mcrr p0, #1, Rt, Rt2, c0 | Simultaneously: sio hw→gpio togl = Rt; sio hw→gpio hi togl = Rt2; _ _ _ _ _ |
| gpioc hilo out set _ _ _ | mcrr p0, #2, Rt, Rt2, c0 | Simultaneously: sio hw→gpio set = Rt; sio hw→gpio hi set = Rt2; _ _ _ _ _ |
| gpioc hilo out clr _ _ _ | mcrr p0, #3, Rt, Rt2, c0 | Simultaneously: sio hw→gpio clr = Rt; sio hw→gpio hi clr = Rt2; _ _ _ _ _ |

3.6.1.2. OE mask write instructions

These instructions write to multiple bits in the SIO GPIO_OE and GPIO_HI_OE registers.

| Mnemonic | Armv8-M Instruction | Operation |
| --- | --- | --- |
| gpioc lo oe put _ _ _ | mcr p0, #0, Rt, c0, c4 | sio hw→gpio oe = Rt; _ _ |
| gpioc lo oe xor _ _ _ | mcr p0, #1, Rt, c0, c4 | sio hw→gpio oe togl = Rt; _ _ _ |
| gpioc lo oe set _ _ _ | mcr p0, #2, Rt, c0, c4 | sio hw→gpio oe set = Rt; _ _ _ |
| gpioc lo oe clr _ _ _ | mcr p0, #3, Rt, c0, c4 | sio hw→gpio oe clr = Rt; _ _ _ |
| gpioc hi oe put _ _ _ | mcr p0, #0, Rt, c0, c5 | sio hw→gpio hi oe = Rt; _ _ _ |
| gpioc hi oe xor _ _ _ | mcr p0, #1, Rt, c0, c5 | sio hw→gpio hi oe togl = Rt; _ _ _ _ |
| gpioc hi oe set _ _ _ | mcr p0, #2, Rt, c0, c5 | sio hw→gpio hi oe set = Rt; _ _ _ _ |
| gpioc hi oe clr _ _ _ | mcr p0, #3, Rt, c0, c5 | sio hw→gpio hi oe clr = Rt; _ _ _ _ |
| gpioc hilo oe put _ _ _ | mcrr p0, #0, Rt, Rt2, c4 | Simultaneously: sio hw→gpio oe = Rt; sio hw→gpio hi oe = Rt2; _ _ _ _ _ |
| gpioc hilo oe xor _ _ _ | mcrr p0, #1, Rt, Rt2, c4 | Simultaneously: sio hw→gpio oe togl = Rt; sio hw→gpio hi oe togl = _ _ _ _ _ _ _ Rt2; |
| gpioc hilo oe set _ _ _ | mcrr p0, #2, Rt, Rt2, c4 | Simultaneously: sio hw→gpio oe set = Rt; sio hw→gpio hi oe set = _ _ _ _ _ _ _ Rt2; |
| gpioc hilo oe clr _ _ _ | mcrr p0, #3, Rt, Rt2, c4 | Simultaneously: sio hw→gpio oe clr = Rt; sio hw→gpio hi oe clr = _ _ _ _ _ _ _ Rt2; |

3.6. Cortex-M33 coprocessors
101

RP2350 Datasheet

3.6.1.3. Single-bit write instructions

These instructions write to a single, indexed bit in either the GPIO_OUT and GPIO_HI_OUT registers, or the GPIO_OE and

GPIO_HI_OE registers.

| Mnemonic | Armv8-M Instruction | Operation |
| --- | --- | --- |
| gpioc bit out put _ _ _ | mcrr p0, #4, Rt, Rt2, c0 | Write a 1-bit value to any output. Equivalent to: if (Rt2 & 1) gpioc hilo out set(1ull << Rt); else gpioc hilo out clr(1ull << Rt); _ _ _ _ _ _ |
| gpioc bit out xor _ _ _ | mcr p0, #5, Rt, c0, c0 | Unconditionally toggle any single output. Equivalent to: gpioc hilo out xor(1ull << Rt); _ _ _ |
| gpioc bit out set _ _ _ | mcr p0, #6, Rt, c0, c0 | Unconditionally set any single output. Equivalent to: gpioc hilo out set(1ull << Rt); _ _ _ |
| gpioc bit out clr _ _ _ | mcr p0, #7, Rt, c0, c0 | Unconditionally clear any single output. Equivalent to: gpioc hilo out clr(1ull << Rt); _ _ _ |
| gpioc bit out xor2 _ _ _ | mcrr p0, #5, Rt, Rt2, c0 | Conditionally toggle any single output. Equivalent to: gpioc hilo out xor((uint64 t)(Rt2 & 1) << Rt); _ _ _ _ |
| gpioc bit out set2 _ _ _ | mcrr p0, #6, Rt, Rt2, c0 | Conditionally set any single output. Equivalent to: gpioc hilo out set((uint64 t)(Rt2 & 1) << Rt); _ _ _ _ |
| gpioc bit out clr2 _ _ _ | mcrr p0, #7, Rt, Rt2, c0 | Conditionally clear any single output. Equivalent to: gpioc hilo out clr((uint64 t)(Rt2 & 1) << Rt); _ _ _ _ |
| gpioc bit oe put _ _ _ | mcrr p0, #4, Rt, Rt2, c4 | Write a 1-bit value to any output enable. Equivalent to: if (Rt2 & 1) gpioc hilo oe set(1ull << Rt); else gpioc hilo oe clr(1ull << Rt); _ _ _ _ _ _ |
| gpioc bit oe xor _ _ _ | mcr p0, #5, Rt, c0, c4 | Unconditionally toggle any output enable. Equivalent to: gpioc hilo oe xor(1ull << Rt); _ _ _ |
| gpioc bit oe set _ _ _ | mcr p0, #6, Rt, c0, c4 | Unconditionally set any output enable (set to output). Equivalent to: gpioc hilo oe set(1ull << Rt); _ _ _ |
| gpioc bit oe clr _ _ _ | mcr p0, #7, Rt, c0, c4 | Unconditionally clear any output enable (set to input). Equivalent to: gpioc hilo oe clr(1ull << Rt); _ _ _ |
| gpioc bit oe xor2 _ _ _ | mcrr p0, #5, Rt, Rt2, c4 | Conditionally toggle any output enable. Equivalent to: gpioc hilo oe xor((uint64 t)(Rt2 & 1) << Rt); _ _ _ _ |
| gpioc bit oe set2 _ _ _ | mcrr p0, #6, Rt, Rt2, c4 | Conditionally set any output enable (set to output). Equivalent to: gpioc hilo oe set((uint64 t)(Rt2 & 1) << Rt); _ _ _ _ |
| gpioc bit oe clr2 _ _ _ | mcrr p0, #7, Rt, Rt2, c4 | Conditionally clear any output enable (set to input). Equivalent to: gpioc hilo oe clr((uint64 t)(Rt2 & 1) << Rt); _ _ _ _ |
| gpioc index out put _ _ _ | mcrr p0, #8, Rt, Rt2, c0 | Write Rt to a GPIO output register selected by Rt2. |
| gpioc index out xor _ _ _ | mcrr p0, #9, Rt, Rt2, c0 | Toggle bits Rt in a GPIO output register selected by Rt2. |
| gpioc index out set _ _ _ | mcrr p0, #10, Rt, Rt2, c0 | Set bits Rt in a GPIO output register selected by Rt2. |
| gpioc index out clr _ _ _ | mcrr p0, #11, Rt, Rt2, c0 | Clear bits Rt in a GPIO output register selected by Rt2. |
| gpioc index oe put _ _ _ | mcrr p0, #8, Rt, Rt2, c4 | Write Rt to a GPIO output enable register selected by Rt2 |
| gpioc index oe xor _ _ _ | mcrr p0, #9, Rt, Rt2, c4 | Toggle bits Rt in a GPIO output enable register selected by Rt2. |
| gpioc index oe set _ _ _ | mcrr p0, #10, Rt, Rt2, c4 | Set bits Rt in a GPIO output enable register selected by Rt2 (i.e. set to output). |
| gpioc index oe clr _ _ _ | mcrr p0, #11, Rt, Rt2, c4 | Clear bits Rt in a GPIO output enable register selected by Rt2 (i.e. set to input). |

3.6. Cortex-M33 coprocessors
102

RP2350 Datasheet

3.6.1.4. Indexed mask write instructions

These instructions write to a single, dynamically selected 32-bit GPIO register.

3.6.1.5. Read instructions

These instructions read from either the GPIO_OUT and GPIO_HI_OUT registers; the GPIO_OE and GPIO_HI_OE registers;

or the GPIO_IN and GPIO_HI_IN registers.

| Mnemonic | Armv8-M Instruction | Operation |
| --- | --- | --- |
| gpioc lo out get _ _ _ | mrc p0, #0, Rt, c0, c0 | Read back the lower 32-bit output register. Equivalent to: Rt = sio hw→gpio out; _ _ |
| gpioc hi out get _ _ _ | mrc p0, #0, Rt, c0, c1 | Read back the upper 32-bit output register. Equivalent to: Rt = sio hw→gpio hi out; _ _ _ |
| gpioc hilo out get _ _ _ | mrrc p0, #0, Rt, Rt2, c0 | Read back two 32-bit output registers in a single operation. Equivalent to: Rt = sio hw→gpio out; and simultaneously Rt2 = _ _ sio hw→gpio hi out << 32); _ _ _ |
| gpioc lo oe get _ _ _ | mrc p0, #0, Rt, c0, c4 | Read back the lower 32-bit output enable register. Equivalent to: Rt = sio hw→gpio oe; _ _ |
| gpioc hi oe get _ _ _ | mrc p0, #0, Rt, c0, c5 | Read back the upper 32-bit output enable register. Equivalent to: Rt = sio hw→gpio hi oe; _ _ _ |
| gpioc hilo oe get _ _ _ | mrrc p0, #0, Rt, Rt2, c4 | Read back two 32-bit output enable registers in a single operation. Equivalent to: Rt = sio hw→gpio oe; and simultaneously Rt2 = _ _ sio hw→gpio hi oe << 32); _ _ _ |
| gpioc lo in get _ _ _ | mrc p0, #0, Rt, c0, c8 | Sample the lower 32 GPIOs. Equivalent to: Rt = sio hw→gpio in; _ _ |
| gpioc hi in get _ _ _ | mrc p0, #0, Rt, c0, c9 | Sample the upper 32 GPIOs. Equivalent to: Rt = sio hw→gpio hi in; _ _ _ |
| gpioc hilo in get _ _ _ | mrrc p0, #0, Rt, Rt2, c8 | Sample 64 GPIOs on the same cycle. Equivalent to: Rt = sio hw→gpio in; and simultaneously Rt2 = sio hw→gpio hi in << 32); _ _ _ _ _ |

3.6.1.6. Interpreting instruction fields

The type of coprocessor instruction — mrc, mrrc, mcr and mcrr — specifies the direction of the transfer (read/write) and the

number of Arm registers being transferred (one or two).

Bits 3:2 of the first coprocessor register number field, CRm, identify the group of registers being accessed. Values 0, 1 and

3.6. Cortex-M33 coprocessors
103

RP2350 Datasheet

2 refer to the output, output enable and input registers respectively.

Bit 0 of the first coprocessor register number field, CRm, may be used to distinguish which register in a group is being

accessed. Bit 1 is reserved to allow more registers to be indexed on future chips with more GPIOs.

For writes, bits 1:0 of the instruction’s opc1 field specify the type of write operation: values 0, 1, 2, 3 map to normal write,

XOR, set and clear operations respectively. Bits 3:2 of the opc1 field are used to indicate the addressing mode for the

register or individual bit being accessed. Their exact interpretation depends on the instruction.

Any combinations not listed in the preceding tables are reserved for future use.

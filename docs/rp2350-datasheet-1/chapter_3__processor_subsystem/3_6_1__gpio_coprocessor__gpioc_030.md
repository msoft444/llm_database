---
source_pdf: rp2350-datasheet-1.pdf
repository: llm_database
chapter: Chapter 3. Processor subsystem
section: 3.6.1. GPIO coprocessor (GPIOC)
pages: 102-104
type: technical_spec
generated_at: 2026-02-28T17:22:56.644477+00:00
---

# 3.6.1. GPIO coprocessor (GPIOC)

Before accessing a coprocessor from Secure code, that coprocessor must first be enabled by setting the corresponding
bit in the CPACR. Before accessing from the Non-secure state, the corresponding bits in the NSACR and CPACR_NS
registers must be set.
The RISC-V processors on RP2350 do not have access to the Cortex-M33 coprocessors.
3.6.1. GPIO coprocessor (GPIOC)
Coprocessor port 0 provides low-overhead access from the Cortex-M33 processors to the GPIO registers in the SIO
(Section 3.1.3). This enables a single coprocessor instruction to sample all 48 GPIOs, or to set/clear/write any single
GPIO, among other functionality.
Non-secure accesses are filtered according to the GPIO_NSMASK0 and GPIO_NSMASK1 registers in ACCESSCTRL.
GPIOs not granted for Non-secure use will ignore writes from the Non-secure state, and read back as zero when read
from the Non-secure state.
3.6.1.1. OUT mask write instructions
These instructions write to multiple bits in the SIO GPIO_OUT and GPIO_HI_OUT registers.
Mnemonic
Armv8-M Instruction
Operation
gpioc_lo_out_put
mcr p0, #0, Rt, c0, c0
sio_hw→gpio_out = Rt;
gpioc_lo_out_xor
mcr p0, #1, Rt, c0, c0
sio_hw→gpio_togl = Rt;
gpioc_lo_out_set
mcr p0, #2, Rt, c0, c0
sio_hw→gpio_set = Rt;
gpioc_lo_out_clr
mcr p0, #3, Rt, c0, c0
sio_hw→gpio_clr = Rt;
gpioc_hi_out_put
mcr p0, #0, Rt, c0, c1
sio_hw→gpio_hi_out = Rt;
gpioc_hi_out_xor
mcr p0, #1, Rt, c0, c1
sio_hw→gpio_hi_togl = Rt;
gpioc_hi_out_set
mcr p0, #2, Rt, c0, c1
sio_hw→gpio_hi_set = Rt;
gpioc_hi_out_clr
mcr p0, #3, Rt, c0, c1
sio_hw→gpio_hi_clr = Rt;
gpioc_hilo_out_put
mcrr p0, #0, Rt, Rt2, c0
Simultaneously: sio_hw→gpio_out = Rt; sio_hw→gpio_hi_out = Rt2;
gpioc_hilo_out_xor
mcrr p0, #1, Rt, Rt2, c0
Simultaneously: sio_hw→gpio_togl = Rt; sio_hw→gpio_hi_togl = Rt2;
gpioc_hilo_out_set
mcrr p0, #2, Rt, Rt2, c0
Simultaneously: sio_hw→gpio_set = Rt; sio_hw→gpio_hi_set = Rt2;
gpioc_hilo_out_clr
mcrr p0, #3, Rt, Rt2, c0
Simultaneously: sio_hw→gpio_clr = Rt; sio_hw→gpio_hi_clr = Rt2;
3.6.1.2. OE mask write instructions
These instructions write to multiple bits in the SIO GPIO_OE and GPIO_HI_OE registers.
Mnemonic
Armv8-M Instruction
Operation
gpioc_lo_oe_put
mcr p0, #0, Rt, c0, c4
sio_hw→gpio_oe = Rt;
gpioc_lo_oe_xor
mcr p0, #1, Rt, c0, c4
sio_hw→gpio_oe_togl = Rt;
gpioc_lo_oe_set
mcr p0, #2, Rt, c0, c4
sio_hw→gpio_oe_set = Rt;
gpioc_lo_oe_clr
mcr p0, #3, Rt, c0, c4
sio_hw→gpio_oe_clr = Rt;
gpioc_hi_oe_put
mcr p0, #0, Rt, c0, c5
sio_hw→gpio_hi_oe = Rt;
gpioc_hi_oe_xor
mcr p0, #1, Rt, c0, c5
sio_hw→gpio_hi_oe_togl = Rt;
RP2350 Datasheet
3.6. Cortex-M33 coprocessors
101


Mnemonic
Armv8-M Instruction
Operation
gpioc_hi_oe_set
mcr p0, #2, Rt, c0, c5
sio_hw→gpio_hi_oe_set = Rt;
gpioc_hi_oe_clr
mcr p0, #3, Rt, c0, c5
sio_hw→gpio_hi_oe_clr = Rt;
gpioc_hilo_oe_put
mcrr p0, #0, Rt, Rt2, c4
Simultaneously: sio_hw→gpio_oe = Rt; sio_hw→gpio_hi_oe = Rt2;
gpioc_hilo_oe_xor
mcrr p0, #1, Rt, Rt2, c4
Simultaneously: sio_hw→gpio_oe_togl = Rt; sio_hw→gpio_hi_oe_togl =
Rt2;
gpioc_hilo_oe_set
mcrr p0, #2, Rt, Rt2, c4
Simultaneously: sio_hw→gpio_oe_set = Rt; sio_hw→gpio_hi_oe_set =
Rt2;
gpioc_hilo_oe_clr
mcrr p0, #3, Rt, Rt2, c4
Simultaneously: sio_hw→gpio_oe_clr = Rt; sio_hw→gpio_hi_oe_clr =
Rt2;
3.6.1.3. Single-bit write instructions
These instructions write to a single, indexed bit in either the GPIO_OUT and GPIO_HI_OUT registers, or the GPIO_OE and
GPIO_HI_OE registers.
Mnemonic
Armv8-M Instruction
Operation
gpioc_bit_out_put
mcrr p0, #4, Rt, Rt2, c0
Write a 1-bit value to any output. Equivalent to: if (Rt2 & 1)
gpioc_hilo_out_set(1ull << Rt); else gpioc_hilo_out_clr(1ull << Rt);
gpioc_bit_out_xor
mcr p0, #5, Rt, c0, c0
Unconditionally toggle any single output. Equivalent to:
gpioc_hilo_out_xor(1ull << Rt);
gpioc_bit_out_set
mcr p0, #6, Rt, c0, c0
Unconditionally set any single output. Equivalent to:
gpioc_hilo_out_set(1ull << Rt);
gpioc_bit_out_clr
mcr p0, #7, Rt, c0, c0
Unconditionally clear any single output. Equivalent to:
gpioc_hilo_out_clr(1ull << Rt);
gpioc_bit_out_xor2
mcrr p0, #5, Rt, Rt2, c0
Conditionally toggle any single output. Equivalent to:
gpioc_hilo_out_xor((uint64_t)(Rt2 & 1) << Rt);
gpioc_bit_out_set2
mcrr p0, #6, Rt, Rt2, c0
Conditionally set any single output. Equivalent to:
gpioc_hilo_out_set((uint64_t)(Rt2 & 1) << Rt);
gpioc_bit_out_clr2
mcrr p0, #7, Rt, Rt2, c0
Conditionally clear any single output. Equivalent to:
gpioc_hilo_out_clr((uint64_t)(Rt2 & 1) << Rt);
gpioc_bit_oe_put
mcrr p0, #4, Rt, Rt2, c4
Write a 1-bit value to any output enable. Equivalent to: if (Rt2 & 1)
gpioc_hilo_oe_set(1ull << Rt); else gpioc_hilo_oe_clr(1ull << Rt);
gpioc_bit_oe_xor
mcr p0, #5, Rt, c0, c4
Unconditionally toggle any output enable. Equivalent to:
gpioc_hilo_oe_xor(1ull << Rt);
gpioc_bit_oe_set
mcr p0, #6, Rt, c0, c4
Unconditionally set any output enable (set to output). Equivalent to:
gpioc_hilo_oe_set(1ull << Rt);
gpioc_bit_oe_clr
mcr p0, #7, Rt, c0, c4
Unconditionally clear any output enable (set to input). Equivalent to:
gpioc_hilo_oe_clr(1ull << Rt);
gpioc_bit_oe_xor2
mcrr p0, #5, Rt, Rt2, c4
Conditionally toggle any output enable. Equivalent to:
gpioc_hilo_oe_xor((uint64_t)(Rt2 & 1) << Rt);
gpioc_bit_oe_set2
mcrr p0, #6, Rt, Rt2, c4
Conditionally set any output enable (set to output). Equivalent to:
gpioc_hilo_oe_set((uint64_t)(Rt2 & 1) << Rt);
gpioc_bit_oe_clr2
mcrr p0, #7, Rt, Rt2, c4
Conditionally clear any output enable (set to input). Equivalent to:
gpioc_hilo_oe_clr((uint64_t)(Rt2 & 1) << Rt);
RP2350 Datasheet
3.6. Cortex-M33 coprocessors
102


3.6.1.4. Indexed mask write instructions
These instructions write to a single, dynamically selected 32-bit GPIO register.
Mnemonic
Armv8-M Instruction
Operation
gpioc_index_out_put
mcrr p0, #8, Rt, Rt2, c0
Write Rt to a GPIO output register selected by Rt2.
gpioc_index_out_xor
mcrr p0, #9, Rt, Rt2, c0
Toggle bits Rt in a GPIO output register selected by Rt2.
gpioc_index_out_set
mcrr p0, #10, Rt, Rt2, c0
Set bits Rt in a GPIO output register selected by Rt2.
gpioc_index_out_clr
mcrr p0, #11, Rt, Rt2, c0
Clear bits Rt in a GPIO output register selected by Rt2.
gpioc_index_oe_put
mcrr p0, #8, Rt, Rt2, c4
Write Rt to a GPIO output enable register selected by Rt2
gpioc_index_oe_xor
mcrr p0, #9, Rt, Rt2, c4
Toggle bits Rt in a GPIO output enable register selected by Rt2.
gpioc_index_oe_set
mcrr p0, #10, Rt, Rt2, c4
Set bits Rt in a GPIO output enable register selected by Rt2 (i.e. set
to output).
gpioc_index_oe_clr
mcrr p0, #11, Rt, Rt2, c4
Clear bits Rt in a GPIO output enable register selected by Rt2 (i.e. set
to input).
3.6.1.5. Read instructions
These instructions read from either the GPIO_OUT and GPIO_HI_OUT registers; the GPIO_OE and GPIO_HI_OE registers;
or the GPIO_IN and GPIO_HI_IN registers.
Mnemonic
Armv8-M Instruction
Operation
gpioc_lo_out_get
mrc p0, #0, Rt, c0, c0
Read back the lower 32-bit output register. Equivalent to: Rt =
sio_hw→gpio_out;
gpioc_hi_out_get
mrc p0, #0, Rt, c0, c1
Read back the upper 32-bit output register. Equivalent to: Rt =
sio_hw→gpio_hi_out;
gpioc_hilo_out_get
mrrc p0, #0, Rt, Rt2, c0
Read back two 32-bit output registers in a single operation.
Equivalent to: Rt = sio_hw→gpio_out; and simultaneously Rt2 =
sio_hw→gpio_hi_out << 32);
gpioc_lo_oe_get
mrc p0, #0, Rt, c0, c4
Read back the lower 32-bit output enable register. Equivalent to: Rt
= sio_hw→gpio_oe;
gpioc_hi_oe_get
mrc p0, #0, Rt, c0, c5
Read back the upper 32-bit output enable register. Equivalent to: Rt
= sio_hw→gpio_hi_oe;
gpioc_hilo_oe_get
mrrc p0, #0, Rt, Rt2, c4
Read back two 32-bit output enable registers in a single operation.
Equivalent to: Rt = sio_hw→gpio_oe; and simultaneously Rt2 =
sio_hw→gpio_hi_oe << 32);
gpioc_lo_in_get
mrc p0, #0, Rt, c0, c8
Sample the lower 32 GPIOs. Equivalent to: Rt = sio_hw→gpio_in;
gpioc_hi_in_get
mrc p0, #0, Rt, c0, c9
Sample the upper 32 GPIOs. Equivalent to: Rt = sio_hw→gpio_hi_in;
gpioc_hilo_in_get
mrrc p0, #0, Rt, Rt2, c8
Sample 64 GPIOs on the same cycle. Equivalent to: Rt =
sio_hw→gpio_in; and simultaneously Rt2 = sio_hw→gpio_hi_in << 32);
3.6.1.6. Interpreting instruction fields
The type of coprocessor instruction — mrc, mrrc, mcr and mcrr — specifies the direction of the transfer (read/write) and the
number of Arm registers being transferred (one or two).
Bits 3:2 of the first coprocessor register number field, CRm, identify the group of registers being accessed. Values 0, 1 and
RP2350 Datasheet
3.6. Cortex-M33 coprocessors
103


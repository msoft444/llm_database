---
source_pdf: rp2350-datasheet-1.pdf
repository: llm_database
chapter: Chapter 3. Processor subsystem
section: 3.8.8. Configuration
pages: 303-304
type: technical_spec
generated_at: 2026-02-28T17:22:56.644477+00:00
---

# 3.8.8. Configuration

• There are no bus stalls on the instruction fetch port
This is because the branch predictor lookup functions by comparing bits 31:2 of the sequential-fetch counter to the BTB
tag. In this case the BTB tag points to the same word as the loop entry. In the aforementioned case the sequential-fetch
counter never actually contains the address of the loop entry, because the loop entry address goes straight to the bus,
and the sequential-fetch counter pre-increments to the next address. This manifests in delay loops like the following:
.p2align 2
delay_loop_bad_dont_copy_paste_this:
    addi a0, a0, -1
    bgez a0, delay_loop_bad_dont_copy_paste_this
Given the description in Section 3.8.7.10, you might expect this loop to execute at two cycles per iteration in the steady
state. The actual behaviour is it executes at three cycles per iteration until instruction fetch encounters a stall,
whereupon it accelerates to two cycles per instruction until the loop ends.
Avoid this by using a 32-bit instruction in the loop body. Force 32-bit alignment of the loop body to avoid an alignment
penalty. The following code executes at the expected two cycles per iteration in the steady state:
.p2align 2          // Force 4-byte alignment
delay_cycles:
.option push
.option norvc       // Force 32-bit opcode
    addi a0, a0, -1
.option pop
    bgez a0, delay_cycles
3.8.8. Configuration
Hazard3 uses the parameters given in the hazard3_config.vh header to customise the core. These values are set before
taping out a Hazard3 instance on silicon, so they are fixed from a user point of view. They determine which instructions
the processor supports, the area-performance trade-off for certain instructions, and static configuration for core
peripherals like the PMP. RP2350 uses the following values for these parameters:
Parameter
Value
EXTENSION_A
1
EXTENSION_C
1
EXTENSION_M
1
EXTENSION_ZBA
1
EXTENSION_ZBB
1
EXTENSION_ZBC
0
EXTENSION_ZBS
1
EXTENSION_ZCB
1
EXTENSION_ZCMP
1
EXTENSION_ZBKB
1
EXTENSION_ZIFENCEI
1
EXTENSION_XH3BEXTM
1
EXTENSION_XH3IRQ
1
RP2350 Datasheet
3.8. Hazard3 processor
302


Parameter
Value
EXTENSION_XH3PMPM
1
EXTENSION_XH3POWER
1
CSR_M_MANDATORY
1
CSR_M_TRAP
1
CSR_COUNTER
1
U_MODE
1
PMP_REGIONS
11
PMP_GRAIN
3
PMP_HARDWIRED
11’h700
PMP_HARDWIRED_ADDR
See Section 3.8.8.1
PMP_HARDWIRED_CFG
See Section 3.8.8.1
DEBUG_SUPPORT
1
BREAKPOINT_TRIGGERS
4
NUM_IRQS
52
IRQ_PRIORITY_BITS
4
IRQ_INPUT_BYPASS
{NUM_IRQS{1’b1}}
MVENDORID_VAL
32’h00000493
MIMPID_VAL
32’h86fc4e3f
MCONFIGPTR_VAL
32’h0
REDUCED_BYPASS
0
MULDIV_UNROLL
2
MUL_FAST
1
MUL_FASTER
1
MULH_FAST
1
FAST_BRANCHCMP
1
RESET_REGFILE
1
BRANCH_PREDICTOR
1
MTVEC_WMASK
32’hfffffffd
3.8.8.1. Hardwired PMP regions
RP2350 configures Hazard3 with eight dynamically configured PMP regions, and three static ones. The static regions
provide default U-mode RWX permissions on the following ranges:
• ROM: 0x00000000 through 0x0fffffff
• Peripherals: 0x40000000 through 0x5fffffff
• SIO: 0xd0000000 through 0xdfffffff
These addresses appear in PMPADDR8, PMPADDR9 and PMPADDR10. The hardwired PMP address registers behave
the same as dynamic registers, except that they ignore writes (exercising the WARL rule). The permissions for these
RP2350 Datasheet
3.8. Hazard3 processor
303


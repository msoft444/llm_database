---
source_pdf: rp2350-datasheet-9.pdf
repository: llm_database
chapter: Chapter 3. Processor subsystem
section: 3.8.8. Configuration
pages: 303-305
type: technical_spec
generated_at: 2026-03-01T11:44:31.548839+00:00
---

# 3.8.8. Configuration

3.8.8. Configuration

Hazard3 uses the parameters given in the hazard3_config.vh header to customise the core. These values are set before

taping out a Hazard3 instance on silicon, so they are fixed from a user point of view. They determine which instructions

the processor supports, the area-performance trade-off for certain instructions, and static configuration for core

peripherals like the PMP. RP2350 uses the following values for these parameters:

| Parameter | Value |
| --- | --- |
| EXTENSION A _ | 1 |
| EXTENSION C _ | 1 |
| EXTENSION M _ | 1 |
| EXTENSION ZBA _ | 1 |
| EXTENSION ZBB _ | 1 |
| EXTENSION ZBC _ | 0 |
| EXTENSION ZBS _ | 1 |
| EXTENSION ZCB _ | 1 |
| EXTENSION ZCMP _ | 1 |
| EXTENSION ZBKB _ | 1 |
| EXTENSION ZIFENCEI _ | 1 |
| EXTENSION XH3BEXTM _ | 1 |
| EXTENSION XH3IRQ _ | 1 |
| EXTENSION XH3PMPM _ | 1 |
| EXTENSION XH3POWER _ | 1 |
| CSR M MANDATORY _ _ | 1 |
| CSR M TRAP _ _ | 1 |
| CSR COUNTER _ | 1 |
| U MODE _ | 1 |
| PMP REGIONS _ | 11 |
| PMP GRAIN _ | 3 |
| PMP HARDWIRED _ | 11’h700 |
| PMP HARDWIRED ADDR _ _ | See Section 3.8.8.1 |
| PMP HARDWIRED CFG _ _ | See Section 3.8.8.1 |
| DEBUG SUPPORT _ | 1 |
| BREAKPOINT TRIGGERS _ | 4 |
| NUM IRQS _ | 52 |
| IRQ PRIORITY BITS _ _ | 4 |
| IRQ INPUT BYPASS _ _ | {NUM IRQS{1’b1}} _ |
| MVENDORID VAL _ | 32’h00000493 |
| MIMPID VAL _ | 32’h86fc4e3f |
| MCONFIGPTR VAL _ | 32’h0 |
| REDUCED BYPASS _ | 0 |
| MULDIV UNROLL _ | 2 |
| MUL FAST _ | 1 |
| MUL FASTER _ | 1 |
| MULH FAST _ | 1 |
| FAST BRANCHCMP _ | 1 |
| RESET REGFILE _ | 1 |
| BRANCH PREDICTOR _ | 1 |
| MTVEC WMASK _ | 32’hfffffffd |

3.8. Hazard3 processor
302

RP2350 Datasheet

3.8.8.1. Hardwired PMP regions

RP2350 configures Hazard3 with eight dynamically configured PMP regions, and three static ones. The static regions

provide default U-mode RWX permissions on the following ranges:

• ROM: 0x00000000 through 0x0fffffff
• Peripherals: 0x40000000 through 0x5fffffff
• SIO: 0xd0000000 through 0xdfffffff

These addresses appear in PMPADDR8, PMPADDR9 and PMPADDR10. The hardwired PMP address registers behave

the same as dynamic registers, except that they ignore writes (exercising the WARL rule). The permissions for these

3.8. Hazard3 processor
303

RP2350 Datasheet

regions are in PMPCFG2.

The hardwired regions have a similar role to the Exempt regions added to the Cortex-M33 IDAU address map specified

in Section 10.2.2.

RP2350 puts default U-mode permissions on AHB/APB peripherals because these are expected to be assigned using

ACCESSCTRL (Section 10.6). ACCESSCTRL can assign each peripheral individually, using the existing address decoders

in the bus fabric, whereas PMP regions are in limited supply so are less useful for peripheral assignment.

Similarly, SIO has internal banking over Secure/Non-secure bus attribution, which is mapped onto Machine and User

modes as described in Section 10.6.2.

The dynamic regions 0 through 7 take priority over the hardwired regions, because the PMP prioritises lower-numbered

regions.

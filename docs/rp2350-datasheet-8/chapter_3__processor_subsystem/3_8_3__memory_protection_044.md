---
source_pdf: rp2350-datasheet-8.pdf
repository: llm_database
chapter: Chapter 3. Processor subsystem
section: 3.8.3. Memory protection
pages: 280-283
type: technical_spec
generated_at: 2026-03-01T08:45:43.944040+00:00
---

# 3.8.3. Memory protection

3.8.3. Memory protection

Hazard3 implements Physical Memory Protection (PMP). It does not implement the Sv32 virtual memory extension or

its associated protections.

The PMP defines permissions for physical addresses. It mostly protects M-mode memory from S-mode and U-mode

access. Hazard3 only implements M-mode and U-mode.

A PMP region applies read, write and execute permissions to a span of byte addresses. For each region there is one

address register, PMPADDR0 through PMPADDR15, and an 8-bit configuration field packed into PMPCFG0 through

PMPCFG3. The read, write and execute permissions are always enforced for U-mode. They may also be enforced for M-

mode, depending on the PMPCFG L bit for that region, and the PMPCFGM0 register.

RP2350 configures Hazard3’s PMP hardware with the following features:

3.8. Hazard3 processor
279

RP2350 Datasheet

• 8× dynamically configurable regions, 0 through 7
• 3× statically configured (hardwired) regions, 8 through 10
• (Remaining regions 11 through 15 are hardwired to OFF)
• A granule of 32 bytes
• Support for naturally aligned power of two (NAPOT) region shapes only
• The custom PMPCFGM0 CSR can apply M-mode permissions to individual regions without locking them

Section 3.8.8.1 defines the configuration of the hardwired regions 8 through 10. These regions apply default U-mode

permissions to RP2350 ROM and peripherals, to avoid having to spend dynamic regions to cover these addresses. The

system-level ACCESSCTRL registers (Section 10.6) can assign each peripheral individually to M-mode or U-mode.

When multiple PMP regions match the same byte address, the lowest-numbered of these regions takes effect. The

other regions are ignored.

3.8.3.1. PMP address registers

Addresses in PMP address registers PMPADDR0 through PMPADDR15 are stored with a right-shift of two, so that they

can cover a 16 GB physical address space when Sv32 address translation is in effect. Hazard3 does not implement

address translation, so the physical address space is 4 GB (32-bit byte-addressed) and the two MSBs of each address

register are hardwired to zero.

The RP2350 configuration of Hazard3 supports only the OFF and NAPOT values for the PMPCFG A fields (e.g.

PMPCFG0.R0_A). Setting A to OFF means the region matches no bytes, and is effectively disabled. Setting A to NAPOT

means the region matches on a naturally aligned span of bytes (the base address modulo the size is zero) whose size is

a power of two.

The number of trailing 1s in the PMP address value encodes the size of an NAPOT region. This is the number of

consecutive 1s counted from the LSB without reaching a 0. A PMP address value with no trailing ones (ending in a 0)

matches a region eight bytes in size, and the region size is doubled with each additional 1 bit.

The PMP region matches on the address bits to the left of the least-significant 0 bit. Because the PMP address registers

are right-shifted by two, you must apply the same shift to the addresses being compared. The following examples

demonstrate how to match addresses based on PMPADDRx values:

• The 30-bit all-ones bit pattern 0x3fffffff has the maximum possible size, and matches all addresses.
• The all-zeroes bit pattern 0x00000000 has the minimum possible size.

◦Since there are no trailing 1s, this matches starting from bit 1 of the PMP address register.

◦Due to addresses being right-shifted by two, this is a region of eight bytes starting from address 0x0.
• The bit pattern 0x???????7 (where ? is any digit) matches any 64-byte region.

◦Shift the base address of this 64-byte region by two to get bits 29:4 of the PMPADDRx value.
• The bit pattern 0x0800000f matches byte addresses between 0x20000000 and 0x2000007f, the first 128 bytes of SRAM.

◦Right-shift the base address (0x20000000) by two to get 0x08000000.

◦Add trailing ones to increase the region size and get the final value of 0x0800000f.

◦The size of the region is eight bytes times two to the power of the number of trailing 1 bits, which in this case

(four 1s) works out to 8 × 24 = 128 bytes.

For more examples of PMP address match patterns, see the hardwired PMP region values in Section 3.8.8.1.

RP2350 configures Hazard3 with a granule of 32 bytes. This means the two least-significant bits of each PMP address

register are hardwired to all-ones when the region is enabled. The hardware does not decode address regions smaller

than 32 bytes.

3.8. Hazard3 processor
280

RP2350 Datasheet

3.8.3.2. PMP permissions

Each 8-bit PMP configuration field contains three permission flags:

• R permits non-instruction-fetch reads:

◦load instructions

◦the read phase of AMOs
• W permits writes:

◦store instructions

◦the write phase of AMOs
• X permits instruction execution

A 1 value for each permission means it is granted, and a 0 means it is revoked. These permissions apply to U-mode

access to the region. They also apply to M-mode accesses when any of the following is true:

• The L (lock) configuration bit is 1
• The Hazard3 custom PMPCFGM0 register bit for this region is 1

The L (lock) bit also locks the associated PMP address register and 8-bit PMP configuration field, so that it ignores

future writes. You should always lock PMP regions consecutively from region 0, so that locked regions cannot be

bypassed by unlocked regions.

U-mode accesses that match no PMP regions have no permissions: all memory accesses fail. M-mode accesses that

match no PMP regions have all permissions. The hardwired PMP regions in Section 3.8.8.1 define additional U-mode

permissions for the ROM and peripheral address ranges: these can be overridden by enabling any of the dynamically

configured regions.

NOTE

Due to RP2350-E6 the field order in the PMP configuration fields is R, W, X (MSB-first) rather than the standard X, W, R.

The SDK register headers match the as-implemented order.

3.8.3.3. Accesses spanning multiple PMP regions

Hazard3 does not support non-naturally-aligned loads or stores, other than to generate standard exceptions when they

are attempted. Since NAPOT PMP regions are always naturally aligned, it is impossible for a load or store to span two

PMP regions. Therefore, all bytes covered by a load or store instruction are determined by at most a single active PMP

region that matches the lowest byte address accessed by that instruction.

Instructions are up to 32 bits in size with as little as 16-bit alignment. Therefore it is possible for an instruction to match

multiple PMP regions. When this happens, the instruction generates an instruction fault exception, (mcause = 0x1), unless

there is a lower-numbered PMP region that fully covers the instruction. Lower-numbered PMP regions take precedence.

The exact quote from the privileged ISA specification is: "The lowest-numbered PMP entry that matches any byte of an

access determines whether that access succeeds or fails. The matching PMP entry must match all bytes of an access, or

the access fails, irrespective of the L, R, W, and X bits." (page 60 of RISC-V privileged ISA manual version 20211203).

The RISC-V specification is flexible in what is considered a single access for the purposes of memory protection

checking. Hazard3 considers the fetch of one instruction to be a single access. It therefore forbids instruction fetches

that straddle two PMP regions, even if both regions grant execute permission. Due to this architecture rule, portable

RISC-V software must not assume it can execute instructions that span multiple PMP regions.

Avoid this issue by using hole-punching region configurations in preference to glueing configurations. Suppose you want

to cover the first 12 kB of SRAM (0x20000000 → 0x20002fff), this can be achieved in two ways:

• One region adding permissions to 0x20000000 → 0x200001fff, and another region adding permissions to 0x20002000 →

0x20002fff

3.8. Hazard3 processor
281

RP2350 Datasheet

• One region adding permissions to 0x20000000 → 0x20003fff, and a lower-numbered region subtracting permissions

from 0x20003000 → 0x20003fff

The former option has a crack between the two regions, which has potentially unwanted effects on some platforms. The

latter avoids this issue entirely.

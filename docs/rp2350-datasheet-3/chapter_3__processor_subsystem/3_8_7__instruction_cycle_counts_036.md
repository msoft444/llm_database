---
source_pdf: rp2350-datasheet-3.pdf
repository: llm_database
chapter: Chapter 3. Processor subsystem
section: 3.8.7. Instruction cycle counts
pages: 297-302
type: technical_spec
generated_at: 2026-03-01T02:30:35.833805+00:00
---

# 3.8.7. Instruction cycle counts

![Page 297 figure](images/fig_p0297.png)

RP2350 Datasheet

#endif

.endm

3.8.7. Instruction cycle counts

All timings are given assuming perfect bus behaviour (no downstream bus stalls).

See Section 3.8.1.6 for a synopsis of instruction behaviour.

3.8.7.1. RV32I

Instruction
Cycles
Note

Integer Register-register

add rd, rs1, rs2
1

sub rd, rs1, rs2
1

slt rd, rs1, rs2
1

sltu rd, rs1, rs2
1

and rd, rs1, rs2
1

or rd, rs1, rs2
1

xor rd, rs1, rs2
1

sll rd, rs1, rs2
1

srl rd, rs1, rs2
1

sra rd, rs1, rs2
1

Integer Register-immediate

addi rd, rs1, imm
1
nop is a pseudo-op for addi x0, x0, 0

slti rd, rs1, imm
1

sltiu rd, rs1, imm
1

andi rd, rs1, imm
1

ori rd, rs1, imm
1

xori rd, rs1, imm
1

slli rd, rs1, imm
1

srli rd, rs1, imm
1

srai rd, rs1, imm
1

Large Immediate

lui rd, imm
1

auipc rd, imm
1

Control Transfer

jal rd, label
2[1]

jalr rd, rs1, imm
2[1]

3.8. Hazard3 processor
296

![Page 298 figure](images/fig_p0298.png)

RP2350 Datasheet

| Instruction | Cycles | Note |
| --- | --- | --- |
| beq rs1, rs2, label | 1 or 2[1] | 1 if correctly predicted, 2 if mispredicted. |
| bne rs1, rs2, label | 1 or 2[1] | 1 if correctly predicted, 2 if mispredicted. |
| blt rs1, rs2, label | 1 or 2[1] | 1 if correctly predicted, 2 if mispredicted. |
| bge rs1, rs2, label | 1 or 2[1] | 1 if correctly predicted, 2 if mispredicted. |
| bltu rs1, rs2, label | 1 or 2[1] | 1 if correctly predicted, 2 if mispredicted. |
| bgeu rs1, rs2, label | 1 or 2[1] | 1 if correctly predicted, 2 if mispredicted. |
| Load and Store |  |  |
| lw rd, imm(rs1) | 1 or 2 | 1 if next instruction is independent, 2 if dependent.[2] |
| lh rd, imm(rs1) | 1 or 2 | 1 if next instruction is independent, 2 if dependent.[2] |
| lhu rd, imm(rs1) | 1 or 2 | 1 if next instruction is independent, 2 if dependent.[2] |
| lb rd, imm(rs1) | 1 or 2 | 1 if next instruction is independent, 2 if dependent.[2] |
| lbu rd, imm(rs1) | 1 or 2 | 1 if next instruction is independent, 2 if dependent.[2] |
| sw rs2, imm(rs1) | 1 |  |
| sh rs2, imm(rs1) | 1 |  |
| sb rs2, imm(rs1) | 1 |  |

3.8.7.2. M extension

Instruction
Cycles
Note

32 × 32 → 32 Multiply

mul rd, rs1, rs2
1

32 × 32 → 64 Multiply, Upper Half

mulh rd, rs1, rs2
1

mulhsu rd, rs1, rs2
1

mulhu rd, rs1, rs2
1

Divide and Remainder

div rd, rs1, rs2
18 or 19
Depending on sign correction

divu rd, rs1, rs2
18

rem rd, rs1, rs2
18 or 19
Depending on sign correction

remu rd, rs1, rs2
18

3.8.7.3. A extension

| Instruction | Cycles | Note |
| --- | --- | --- |
| Load-Reserved/Store-Conditional |  |  |
| lr.w rd, (rs1) | 1 or 2 | 2 if next instruction is dependent[2], an lr.w, sc.w or amo*.w.[3] |
| sc.w rd, rs2, (rs1) | 1 or 2 | 2 if next instruction is dependent[2], an lr.w, sc.w or amo*.w.[3] |

3.8. Hazard3 processor
297

![Page 299 figure](images/fig_p0299.png)

RP2350 Datasheet

| Instruction | Cycles | Note |
| --- | --- | --- |
| Atomic Memory Operations |  |  |
| amoswap.w rd, rs2, (rs1) | 4+ | 4 per attempt. Multiple attempts if reservation is lost.[4] |
| amoadd.w rd, rs2, (rs1) | 4+ | 4 per attempt. Multiple attempts if reservation is lost.[4] |
| amoxor.w rd, rs2, (rs1) | 4+ | 4 per attempt. Multiple attempts if reservation is lost.[4] |
| amoand.w rd, rs2, (rs1) | 4+ | 4 per attempt. Multiple attempts if reservation is lost.[4] |
| amoor.w rd, rs2, (rs1) | 4+ | 4 per attempt. Multiple attempts if reservation is lost.[4] |
| amomin.w rd, rs2, (rs1) | 4+ | 4 per attempt. Multiple attempts if reservation is lost.[4] |
| amomax.w rd, rs2, (rs1) | 4+ | 4 per attempt. Multiple attempts if reservation is lost.[4] |
| amominu.w rd, rs2, (rs1) | 4+ | 4 per attempt. Multiple attempts if reservation is lost.[4] |
| amomaxu.w rd, rs2, (rs1) | 4+ | 4 per attempt. Multiple attempts if reservation is lost.[4] |

3.8.7.4. C extension

All C extension 16-bit instructions are aliases of base RV32I instructions. On Hazard3, they perform identically to their

32-bit counterparts.

A consequence of the C extension is that 32-bit instructions can be non-naturally-aligned. This has no penalty during

sequential execution, but branching to a 32-bit instruction that is not 32-bit-aligned carries a 1 cycle penalty, because

the instruction fetch is cracked into two naturally-aligned bus accesses.

3.8.7.5. Privileged instructions (including Zicsr)

Instruction
Cycles
Note

CSR Access

csrrw rd, csr, rs1
1

csrrc rd, csr, rs1
1

csrrs rd, csr, rs1
1

csrrwi rd, csr, imm
1

csrrci rd, csr, imm
1

csrrsi rd, csr, imm
1

Traps and Interrupts

ecall
3
Time given is for jumping to mtvec

ebreak
3
Time given is for jumping to mtvec

mret
2[1]

wfi
2+
Always stalls for one cycle, no upper limit

3.8.7.6. Bit manipulation

3.8. Hazard3 processor
298

![Page 300 figure](images/fig_p0300.png)

RP2350 Datasheet

Instruction
Cycles
Note

Zba (address generation)

sh1add rd, rs1, rs2
1

sh2add rd, rs1, rs2
1

sh3add rd, rs1, rs2
1

Zbb (basic bit manipulation)

andn rd, rs1, rs2
1

clz rd, rs1
1

cpop rd, rs1
1

ctz rd, rs1
1

max rd, rs1, rs2
1

maxu rd, rs1, rs2
1

min rd, rs1, rs2
1

minu rd, rs1, rs2
1

orc.b rd, rs1
1

orn rd, rs1, rs2
1

rev8 rd, rs1
1

rol rd, rs1, rs2
1

ror rd, rs1, rs2
1

rori rd, rs1, imm
1

sext.b rd, rs1
1

sext.h rd, rs1
1

xnor rd, rs1, rs2
1

zext.h rd, rs1
1

zext.b rd, rs1
1
zext.b is a pseudo-op for andi rd, rs1, 0xff

Zbs (single-bit manipulation)

bclr rd, rs1, rs2
1

bclri rd, rs1, imm
1

bext rd, rs1, rs2
1

bexti rd, rs1, imm
1

binv rd, rs1, rs2
1

binvi rd, rs1, imm
1

bset rd, rs1, rs2
1

bseti rd, rs1, imm
1

Zbkb (basic bit manipulation for cryptography)

pack rd, rs1, rs2
1

packh rd, rs1, rs2
1

3.8. Hazard3 processor
299

![Page 301 figure](images/fig_p0301.png)

RP2350 Datasheet

Instruction
Cycles
Note

brev8 rd, rs1
1

zip rd, rs1
1

unzip rd, rs1
1

3.8.7.7. Zcb extension

Similarly to the C extension, this extension contains 16-bit variants of common 32-bit instructions:

• RV32I base ISA: lbu, lh, lhu, sb, sh, zext.b (alias of andi), not (alias of xori)
• Zbb extension: sext.b, zext.h, sext.h
• M extension: mul

They perform identically to their 32-bit counterparts.

3.8.7.8. Zcmp extension

| Instruction | Cycles | Note |
| --- | --- | --- |
| cm.push rlist, -imm | 1 + n | n is number of registers in rlist |
| cm.pop rlist, imm | 1 + n | n is number of registers in rlist |
| cm.popret rlist, imm | 4 (n = 1)[5] or 2 + n (n >= 2)[1] | n is number of registers in rlist |
| cm.popretz rlist, imm | 5 (n = 1)[5] or 3 + n (n >= 2)[1] | n is number of registers in rlist |
| cm.mva01s r1s', r2s' | 2 |  |
| cm.mvsa01 r1s', r2s' | 2 |  |

3.8.7.9. Table footnotes

[1]
A jump or branch to a 32-bit instruction that isn’t 32-bit-aligned requires one additional cycle because two

naturally aligned bus cycles are required to fetch the target instruction.

[2]
If an instruction in stage 2 (e.g. an add) uses data from stage 3 (e.g. a lw result), a 1-cycle bubble is inserted

between the pair. A load data → store data dependency is not an example of this, because data is

produced and consumed in stage 3. However, load data → load address would qualify, as would e.g. sc.w

→ beqz.

[3]
AMOs are issued as a paired exclusive read and exclusive write on the bus, at the maximum speed of 2

cycles per access, since the bus does not permit pipelining of exclusive reads/writes. If the write phase

fails due to the global monitor reporting a lost reservation, the instruction loops at a rate of 4 cycles per

loop, until success. If the read reservation is refused by the global monitor, the instruction generates a

Store/AMO Fault exception, to avoid an infinite loop.

[4]
A pipeline bubble is inserted between lr.w/sc.w and an immediately-following lr.w/sc.w/amo*, because the

AHB5 bus standard does not permit pipelined exclusive accesses. A stall would be inserted between lr.w

and sc.w anyhow, so the local monitor can be updated based on the lr.w data phase in time to suppress the

sc.w address phase.

[5]
The single-register variants of cm.popret and cm.popretz take the same number of cycles as the two-register

variants, because of an internal load-use dependency on the loaded return address.

3.8. Hazard3 processor
300

RP2350 Datasheet

3.8.7.10. Branch predictor

Hazard3 includes a minimal branch predictor, to accelerate tight loops:

• The instruction frontend remembers the last taken, backward branch in a single-entry branch target buffer (BTB)
• If the same branch is seen again, it is predicted taken
• All other branches are predicted non-taken
• If the core executes but does not take a predicted-taken branch:

◦The core clears the BTB

◦The branch is predicted non-taken on its next execution

Correctly predicted branches execute in one cycle: the frontend is able to stitch together the two nonsequential fetch

paths so that they appear sequential. Mispredicted branches incur a penalty cycle, since a nonsequential fetch address

must be issued when the branch is executed. Consider the following copy routine:

// a0 is dst pointer

// a1 is src pointer

// a2 is len

copy_data:

    beqz a2, 2f

    add a2, a2, a1

1:

    lbu a3, (a0)

    sb a3, (a1)

    addi a0, a0, 1

    addi a1, a1, 1

    bltu a1, a2, 1b

2:

    ret

In the steady state this executes at 5 cycles per loop:

• One cycle for the load
• One cycle for the store: though it depends on the load, the dependency is within stage 3 so there is no stall
• One cycle for each add
• One cycle for the repeatedly-taken backward branch

Without the branch predictor the throughput is 6 cycles per loop. The branch predictor increases the throughput by 20%,

and also reduces energy dissipation due to wasted instruction fetch (memory access is a large fraction of the

instruction energy cost for an embedded processor).

For the above example code, a copy of 10 bytes would take 52 cycles:

• The base cost is 5 cycles per iteration, and there are 10 iterations
• The mispredicted, taken branch at the end of the first iteration costs one cycle
• The mispredicted, non-taken branch at the end of the last iteration costs one cycle

3.8.7.10.1. Caveat: delay loops

The branch predictor does not engage when all of the following are true:

• The loop body consists of a single 16-bit instruction (followed by a repeatedly taken backward branch)
• The loop body is 32-bit-aligned

3.8. Hazard3 processor
301

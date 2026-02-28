---
source_pdf: rp2350-datasheet-1.pdf
repository: llm_database
chapter: Chapter 11. PIO
section: 11.3.3. Expressions
pages: 888-888
type: technical_spec
generated_at: 2026-02-28T17:22:56.644477+00:00
---

# 11.3.3. Expressions

.set <count>
If this directive is present, <count> indicates the number of SET bits to be used. This directive affects the default
state machine configuration for a program. This directive is only valid within a program before the first instruction.
.side_set <count> (opt) (pindirs)
If this directive is present, <count> indicates the number of side-set bits to be used. Additionally, opt may be specified
to indicate that a side <value> is optional for instructions (note this requires stealing an extra bit — in addition to the
<count> bits — from those available for the instruction delay). Finally, pindirs may be specified to indicate that the
side set values should be applied to the PINDIRs and not the PINs. This directive is only valid within a program
before the first instruction.
.wrap_target
Place prior to an instruction, this directive specifies the instruction where execution continues due to program
wrapping. This directive is invalid outside of a program, may only be used once within a program, and if not
specified defaults to the start of the program.
.wrap
Placed after an instruction, this directive specifies the instruction after which, in normal control flow (i.e. jmp with
false condition, or no jmp), the program wraps (to .wrap_target instruction). This directive is invalid outside of a
program, may only be used once within a program, and if not specified defaults to after the last program
instruction.
.lang_opt <lang> <name> <option>
Specifies an option for the program related to a particular language generator. (See Language Generators in
Raspberry Pi Pico-series C/C++ SDK). This directive is invalid outside of a program.
.word <value>
Stores a raw 16-bit value as an instruction in the program. This directive is invalid outside of a program.
11.3.2. Values
The following types of values can be used to define integer numbers or branch targets:
Table 978. Values in
pioasm, i.e. <value>
integer
An integer value, e.g. 3 or -7.
hex
A hexadecimal value, e.g. 0xf.
binary
A binary value, e.g. 0b1001.
symbol
A value defined by a .define (see pioasm_define).
<label>
The instruction offset of the label within the program. Typically used with a JMP instruction
(see Section 11.4.2).
(<expression>)
An expression to be evaluated; see expressions. Note that the parentheses are necessary.
11.3.3. Expressions
Expressions may be freely used within pioasm values.
Table 979.
Expressions in pioasm
i.e. <expression>
<expression> + <expression>
The sum of two expressions
<expression> - <expression>
The difference of two expressions
<expression> * <expression>
The multiplication of two expressions
<expression> / <expression>
The integer division of two expressions
- <expression>
The negation of another expression
RP2350 Datasheet
11.3. PIO assembler (pioasm)
887


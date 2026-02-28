---
source_pdf: rp2350-datasheet-1.pdf
repository: llm_database
chapter: Chapter 11. PIO
section: 11.3.6. Instructions
pages: 889-889
type: technical_spec
generated_at: 2026-02-28T17:22:56.644477+00:00
---

# 11.3.6. Instructions

<expression> << <expression>
One expression shifted left by another expression
<expression> >> <expression>
One expression shifted right by another expression
:: <expression>
The bit reverse of another expression
<value>
Any value (see Section 11.3.2)
11.3.4. Comments
To create a line comment that ignores all content on a certain line following a certain symbol, use // or ;.
To create a C-style block comment that ignores all content across multiple lines until after a start symbol until an end
symbol appears, use /* to begin the comment and */ to end the comment.
11.3.5. Labels
Labels use the following forms at the start of a line:
<symbol>:
PUBLIC <symbol>:

TIP
A label is really just an automatic .define with a value set to the current program instruction offset. A PUBLIC label is
exposed to the user code in the same way as a PUBLIC .define.
11.3.6. Instructions
All pioasm instructions follow a common pattern:
<instruction> (side <side_set_value>) ([<delay_value>])
where:
<instruction>
An assembly instruction detailed in the following sections. (see Section 11.4)
<side_set_value>
A value (see Section 11.3.2) to apply to the side_set pins at the start of the instruction. Note that
the rules for a side-set value via side <side_set_value> are dependent on the .side_set (see
pioasm_side_set) directive for the program. If no .side_set is specified then the side <side_set_value>
is invalid, if an optional number of sideset pins is specified then side <side_set_value> may be
present, and if a non-optional number of sideset pins is specified, then side <side_set_value> is
required. The <side_set_value> must fit within the number of side-set bits specified in the .side_set
directive.
RP2350 Datasheet
11.3. PIO assembler (pioasm)
888


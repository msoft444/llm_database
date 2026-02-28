---
source_pdf: rp2350-datasheet-2.pdf
repository: llm_database
chapter: Chapter 11. PIO
section: 11.3.6. Instructions
pages: 889-889
type: technical_spec
generated_at: 2026-02-28T17:43:10.557178+00:00
---

# 11.3.6. Instructions

![Page 889 figure](images/fig_p0889.png)

RP2350 Datasheet

| <expression> << <expression> | One expression shifted left by another expression |
| --- | --- |
| <expression> >> <expression> | One expression shifted right by another expression |
| :: <expression> | The bit reverse of another expression |
| <value> | Any value (see Section 11.3.2) |

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

| <instruction> | An assembly instruction detailed in the following sections. (see Section 11.4) |
| --- | --- |
| <side set value>
_ _ | A value (see Section 11.3.2) to apply to the side_set pins at the start of the instruction. Note that
the rules for a side-set value via side <side set value> are dependent on the .side set (see
_ _ _
pioasm side set) directive for the program. If no .side set is specified then the side <side set value>
_ _ _ _ _
is invalid, if an optional number of sideset pins is specified then side <side set value> may be
_ _
present, and if a non-optional number of sideset pins is specified, then side <side set value> is
_ _
required. The <side set value> must fit within the number of side-set bits specified in the .side set
_ _ _
directive. |

11.3. PIO assembler (pioasm)
888

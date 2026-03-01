---
source_pdf: rp2350-datasheet-10.pdf
repository: llm_database
chapter: Chapter 11. PIO
section: 11.3.3. Expressions
pages: 888-889
type: technical_spec
generated_at: 2026-03-01T12:20:14.494106+00:00
---

# 11.3.3. Expressions

11.3.3. Expressions

Expressions may be freely used within pioasm values.

| <expression> + <expression> | The sum of two expressions |
| --- | --- |
| <expression> - <expression> | The difference of two expressions |
| <expression> * <expression> | The multiplication of two expressions |
| <expression> / <expression> | The integer division of two expressions |
| - <expression> | The negation of another expression |
| <expression> << <expression> | One expression shifted left by another expression |
| <expression> >> <expression> | One expression shifted right by another expression |
| :: <expression> | The bit reverse of another expression |
| <value> | Any value (see Section 11.3.2) |

*Table 979. Expressions in pioasm i.e. <expression>*

11.3. PIO assembler (pioasm)
887

RP2350 Datasheet

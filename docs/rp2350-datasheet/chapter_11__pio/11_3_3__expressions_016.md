---
source_pdf: rp2350-datasheet.pdf
repository: llm_database
chapter: Chapter 11. PIO
section: 11.3.3. Expressions
pages: 888-889
type: technical_spec
generated_at: 2026-03-02T14:11:45.471392+00:00
---

# 11.3.3. Expressions

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

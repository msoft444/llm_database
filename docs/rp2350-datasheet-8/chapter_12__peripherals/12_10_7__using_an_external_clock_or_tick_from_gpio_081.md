---
source_pdf: rp2350-datasheet-8.pdf
repository: llm_database
chapter: Chapter 12. Peripherals
section: 12.10.7. Using an external clock or tick from GPIO
pages: 1202-1202
type: technical_spec
generated_at: 2026-03-01T08:45:43.944040+00:00
---

# 12.10.7. Using an external clock or tick from GPIO

12.10.7. Using an external clock or tick from GPIO

The following features use a GPIO as a clock or a tick:

• external 32kHz clock source
• external 1kHz tick
• external 1Hz tick

Only 4 GPIOs are available for these features. You can only select one, because they share the same GPIO selection

logic. The set of 4 GPIOs differs between package types. The selection is controlled by a 2-bit register field.

The AON Timer uses the following GPIOs:

• EXT_TIME_REF.SOURCE_SEL = 0 → GPIO12
• EXT_TIME_REF.SOURCE_SEL = 1 → GPIO20
• EXT_TIME_REF.SOURCE_SEL = 2 → GPIO14
• EXT_TIME_REF.SOURCE_SEL = 3 → GPIO22

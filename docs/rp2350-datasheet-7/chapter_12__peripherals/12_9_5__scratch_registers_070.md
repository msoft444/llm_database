---
source_pdf: rp2350-datasheet-7.pdf
repository: llm_database
chapter: Chapter 12. Peripherals
section: 12.9.5. Scratch registers
pages: 1195-1195
type: technical_spec
generated_at: 2026-03-01T06:07:53.318745+00:00
---

# 12.9.5. Scratch registers

12.9.5. Scratch registers

The watchdog contains eight 32-bit scratch registers that can store information between soft resets of the chip. The

scratch registers reset when:

• the watchdog is used to to trigger a chip level reset
• a rst_n_run event occurs, triggered by toggling the RUN pin or cycling the digital core supply (DVDD)

The bootrom checks the watchdog scratch registers for a magic number on boot. You can use this to soft reset the chip

into user-specified code. See Section 5.2.4 for more information.

NOTE

Additional general-purpose scratch registers are available in POWMAN SCRATCH0 through SCRATCH7. These

registers also survive power cycling the switched core domain.

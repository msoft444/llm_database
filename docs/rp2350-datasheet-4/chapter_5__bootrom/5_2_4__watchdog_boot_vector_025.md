---
source_pdf: rp2350-datasheet-4.pdf
repository: llm_database
chapter: Chapter 5. Bootrom
section: 5.2.4. Watchdog boot vector
pages: 372-372
type: technical_spec
generated_at: 2026-03-01T03:27:49.792955+00:00
---

# 5.2.4. Watchdog boot vector

5.2.4. Watchdog boot vector

Watchdog boot allows users to install their own boot handler, and divert control away from the main boot sequence on

non-POR/BOR resets. It recognises the following values written to the watchdog’s upper scratch registers:

• SCRATCH4: magic number 0xb007c0d3
• SCRATCH5: entry point XORed with magic -0xb007c0d3 (0x4ff83f2d)
• SCRATCH6: stack pointer
• SCRATCH7: entry point

If either of the magic numbers mismatch, watchdog boot does not take place. If the numbers match, the Bootrom

5.2. Processor-controlled boot sequence
371

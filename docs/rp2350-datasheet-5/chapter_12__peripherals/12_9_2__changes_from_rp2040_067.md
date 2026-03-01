---
source_pdf: rp2350-datasheet-5.pdf
repository: llm_database
chapter: Chapter 12. Peripherals
section: 12.9.2. Changes from RP2040
pages: 1194-1194
type: technical_spec
generated_at: 2026-03-01T04:13:03.328193+00:00
---

# 12.9.2. Changes from RP2040

![Page 1194 figure](images/fig_p1194.png)

12.9.2. Changes from RP2040

On RP2040, the watchdog contained a tick generator used to generate a 1μs tick for the watchdog. This was also

distributed to the system timer. On RP2350, the watchdog instead takes a tick input from the system-level ticks block.

See Section 8.5.

As on RP2040 the watchdog can trigger a PSM (Power-on State Machine) sequence to reset system components or it

can be used to reset selected subsystem components. On RP2350, the watchdog can also trigger a chip level reset.

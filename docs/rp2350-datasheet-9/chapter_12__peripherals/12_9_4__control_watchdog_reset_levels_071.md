---
source_pdf: rp2350-datasheet-9.pdf
repository: llm_database
chapter: Chapter 12. Peripherals
section: 12.9.4. Control watchdog reset levels
pages: 1195-1195
type: technical_spec
generated_at: 2026-03-01T11:44:31.548839+00:00
---

# 12.9.4. Control watchdog reset levels

RP2350 Datasheet

12.9.4. Control watchdog reset levels

To control the level of reset triggered by a watchdog event, use the registers outside the watchdog register block:

• POWMAN_WATCHDOG allows the watchdog to trigger chip level resets
• PSM_WDSEL allows the watchdog to trigger system resets by running a full or partial PSM sequence (Power-on State

Machine)
• RESETS_WDSEL allows the watchdog to trigger subsystem resets

These are described in the Resets section, see Chapter 7.

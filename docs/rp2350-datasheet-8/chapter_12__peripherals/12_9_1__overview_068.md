---
source_pdf: rp2350-datasheet-8.pdf
repository: llm_database
chapter: Chapter 12. Peripherals
section: 12.9.1. Overview
pages: 1194-1194
type: technical_spec
generated_at: 2026-03-01T08:45:43.944040+00:00
---

# 12.9.1. Overview

12.9.1. Overview

The watchdog is a countdown timer which can be configured to reset selected components when it reaches zero. In

normal operation it is periodically loaded with a non-zero value to prevent the reset occuring. If the chip locks up or

software gets stuck in a loop, the reset allows recovery.

The watchdog is reset by any chip-level reset (see Section 7.3). The sources of the chip-level reset are:

• Power-On Reset (POR)
• Brown-out Detection (BOD)
• External Reset (from the RUN pin)
• Debugger Reset Request
• Rescue Debug Port Request
• Watchdog - a chip-level reset triggered by the Watchdog will reset the Watchdog
• SWCORE powerdown
• Glitch Detector
• Debugger HZD Reset Request

These are described in Section 7.3.3.

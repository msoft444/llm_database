---
source_pdf: rp2350-datasheet-3.pdf
repository: llm_database
chapter: Chapter 12. Peripherals
section: 12.9.3. Watchdog counter
pages: 1194-1194
type: technical_spec
generated_at: 2026-03-01T02:30:35.833805+00:00
---

# 12.9.3. Watchdog counter

![Page 1194 figure](images/fig_p1194.png)

RP2350 Datasheet

Offset: 0x48

Description

Interrupt status after masking & forcing

| Bits | Description | Type | Reset |
| --- | --- | --- | --- |
| 31:4 | Reserved. | - | - |
| 3 | ALARM_3 | RO | 0x0 |
| 2 | ALARM_2 | RO | 0x0 |
| 1 | ALARM_1 | RO | 0x0 |
| 0 | ALARM_0 | RO | 0x0 |

Table 1246. INTS

12.9. Watchdog

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

12.9.2. Changes from RP2040

On RP2040, the watchdog contained a tick generator used to generate a 1μs tick for the watchdog. This was also

distributed to the system timer. On RP2350, the watchdog instead takes a tick input from the system-level ticks block.

See Section 8.5.

As on RP2040 the watchdog can trigger a PSM (Power-on State Machine) sequence to reset system components or it

can be used to reset selected subsystem components. On RP2350, the watchdog can also trigger a chip level reset.

12.9.3. Watchdog counter

The watchdog counter is loaded by the LOAD register. The current value can be seen in CTRL.TIME.

12.9. Watchdog
1193

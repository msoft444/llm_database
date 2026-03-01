---
source_pdf: rp2350-datasheet-8.pdf
repository: llm_database
chapter: Chapter 6. Power
section: 6.2. Power management
pages: 444-444
type: technical_spec
generated_at: 2026-03-01T08:45:43.944040+00:00
---

# 6.2. Power management

6.2. Power management

RP2350 retains the power control features of RP2040, but extends them by splitting the chip’s digital core into a number

of power domains, which can be selectively powered off. This allows significant power saving in applications where the

chip is not continuously active. This section describes the core power domains and how they are controlled. The legacy

RP2040 power control features still offer useful power savings, and are described in Section 6.5.

Power domains, and transitions between power states, are controlled by a Power manager. The Power manager runs

from either an internal low power oscillator lposc, or the reference clock clk_ref. The device may be configured to power

down under software control and can wakeup on a GPIO or timer event. Configuration of the power manager is via the

POWMAN registers in Section 6.4 .

---
source_pdf: rp2350-datasheet-7.pdf
repository: llm_database
chapter: Chapter 6. Power
section: 6.1.8. Power supply sequencing
pages: 444-444
type: technical_spec
generated_at: 2026-03-01T06:07:53.318745+00:00
---

# 6.1.8. Power supply sequencing

RP2350 Datasheet

6.1.8. Power supply sequencing

With the exception of the two voltage regulator supplies (VREG_VIN and VREG_AVDD), which should be powered up together,

RP2350’s power supplies may be powered up or down in any order. However, small transient currents may flow in the

ADC supply (ADC_AVDD) if it is powered up before, or powered down after, the digital core supply (DVDD). This will not

damage the chip, but can be avoided by powering up DVDD before or at the same time as ADC_AVDD, and powering down

DVDD after or at the same time as ADC_AVDD. In the most common power supply scheme, where the chip is powered from

a single 3.3 V supply, DVDD will be powered up shortly after ADC_AVDD due to the startup time of the on-chip voltage

regulator. This is acceptable behaviour.

6.2. Power management

RP2350 retains the power control features of RP2040, but extends them by splitting the chip’s digital core into a number

of power domains, which can be selectively powered off. This allows significant power saving in applications where the

chip is not continuously active. This section describes the core power domains and how they are controlled. The legacy

RP2040 power control features still offer useful power savings, and are described in Section 6.5.

Power domains, and transitions between power states, are controlled by a Power manager. The Power manager runs

from either an internal low power oscillator lposc, or the reference clock clk_ref. The device may be configured to power

down under software control and can wakeup on a GPIO or timer event. Configuration of the power manager is via the

POWMAN registers in Section 6.4 .

---
source_pdf: rp2350-datasheet-10.pdf
repository: llm_database
chapter: Chapter 6. Power
section: 6.3.6. Over temperature protection
pages: 451-451
type: technical_spec
generated_at: 2026-03-01T12:20:14.494106+00:00
---

# 6.3.6. Over temperature protection

6.3.6. Over temperature protection

The voltage regulator will terminate regulation and disable its power transistors, if the transistor junction temperature

rises above a threshold set by the HT_TH field in the VREG_CTRL register. The regulator will restart regulation when the

transistor junction temperature drops to approximately 20°C below the temperature threshold.

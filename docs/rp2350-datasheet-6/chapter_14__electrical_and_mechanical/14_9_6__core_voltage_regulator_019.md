---
source_pdf: rp2350-datasheet-6.pdf
repository: llm_database
chapter: Chapter 14. Electrical and mechanical
section: 14.9.6. Core voltage regulator
pages: 1345-1346
type: technical_spec
generated_at: 2026-03-01T05:39:21.783275+00:00
---

# 14.9.6. Core voltage regulator

![Page 1345 figure](images/fig_p1345.png)

14.9.6. Core voltage regulator

| Parameter | Description | Min | Typ | Max | Units |
| --- | --- | --- | --- | --- | --- |
| V OUT (normal mode) | regulated output voltage range (normal mode) | 0.55 | 1.1 | 3.3 | V |
| V OUT (low power mode) | regulated output voltage range (low power mode) | 0.55 | 1.1 | 1.3 | V |
| ΔV OUT (normal mode) | voltage deviation from programmed value (normal mode) | -3 |  | +3 | % of selected output voltage |
| ΔV OUT (low power mode) | voltage deviation from programmed value (low power mode) | -9 |  | +9 | % of selected output voltage |
| I MAX (normal mode) | output current (normal mode) |  |  | 200 | mA |
| I MAX (low power mode) | output current (low power mode) |  |  | 1 | mA |
| I LIMIT (normal mode startup) | current limit (normal mode startup) |  | 240 | 300 | mA |
| I LIMIT (normal mode) | current limit (normal mode) | 260 | 500 | 800 | mA |
| I LIMIT (low power mode) | current limit (low power mode) | 5 |  | 25 | mA |
| VOUT_OK TH.ASSERT | VOUT OK assertion _ threshold | 87 | 90 | 93 | % of selected output voltage |

Table 1442. Voltage

Regulator

Specifications

14.9. Electrical specifications
1344

RP2350 Datasheet

![Page 1346 figure](images/fig_p1346.png)

Parameter
Description
Min
Typ
Max
Units

84
87
90
% of selected

VOUT can exceed the maximum core supply (DVDD). While there is a voltage limit to prevent this happening

accidentally, the limit can be disabled under software control. For reliable operation DVDD should not exceed its

Figure 151. Typical

Regulator Efficiency,

Vout=1.1V,

VREG_VIN=3.3V.

## Embedded Images

![img_p1346_00.png](images/img_p1346_00.png)


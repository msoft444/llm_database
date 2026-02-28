---
source_pdf: rp2350-datasheet-2.pdf
repository: llm_database
chapter: Chapter 14. Electrical and mechanical
section: 14.9.6. Core voltage regulator
pages: 1345-1345
type: technical_spec
generated_at: 2026-02-28T17:43:10.557178+00:00
---

# 14.9.6. Core voltage regulator

![Page 1345 figure](images/fig_p1345.png)

RP2350 Datasheet

| Power Supply | Supplies | Min | Typ | Max | Units |
| --- | --- | --- | --- | --- | --- |
| USB OTP VDD
_ _ | USB PHY & OTP | 3.135 | 3.3 | 3.63 | V |
| ADC AVDDc
_ | ADC | 1.62 | 3.3 | 3.63 | V |

a If IOVDD <2.5V, GPIO VOLTAGE_SELECT registers should be adjusted accordingly. See Section 6.1 for details.

b Short term transients should be within +/-100mV.

c ADC performance will be compromised at voltages below 2.97V

NOTE

RP2354 contains an internal 3.3V flash device, therefore QSPI_IOVDD must be 3.3V. Furthermore, if the QSPI pins are to

be used to connect to an additional flash or PSRAM device, then IOVDD must be 3.3V, as a GPIO is used as QSPI

chip select in this case.

14.9.6. Core voltage regulator

| Parameter | Description | Min | Typ | Max | Units |
| --- | --- | --- | --- | --- | --- |
| V
OUT (normal mode) | regulated output
voltage range
(normal mode) | 0.55 | 1.1 | 3.3 | V |
| V
OUT (low power mode) | regulated output
voltage range (low
power mode) | 0.55 | 1.1 | 1.3 | V |
| ΔV
OUT (normal mode) | voltage deviation
from programmed
value (normal
mode) | -3 |  | +3 | % of selected
output voltage |
| ΔV
OUT (low power mode) | voltage deviation
from programmed
value (low power
mode) | -9 |  | +9 | % of selected
output voltage |
| I
MAX (normal mode) | output current
(normal mode) |  |  | 200 | mA |
| I
MAX (low power mode) | output current
(low power mode) |  |  | 1 | mA |
| I
LIMIT (normal mode startup) | current limit
(normal mode
startup) |  | 240 | 300 | mA |
| I
LIMIT (normal mode) | current limit
(normal mode) | 260 | 500 | 800 | mA |
| I
LIMIT (low power mode) | current limit (low
power mode) | 5 |  | 25 | mA |
| VOUT_OK
TH.ASSERT | VOUT OK assertion
_
threshold | 87 | 90 | 93 | % of selected
output voltage |

Table 1442. Voltage

Regulator

Specifications

14.9. Electrical specifications
1344

---
source_pdf: rp2350-datasheet-10.pdf
repository: llm_database
chapter: Chapter 14. Electrical and mechanical
section: 14.9.5. Power supplies
pages: 1344-1345
type: technical_spec
generated_at: 2026-03-01T12:20:14.494106+00:00
---

# 14.9.5. Power supplies

14.9.5. Power supplies

| Power Supply | Supplies | Min | Typ | Max | Units |
| --- | --- | --- | --- | --- | --- |
| IOVDDa | Digital IO | 1.62 | 1.8 / 3.3 | 3.63 | V |
| QSPI IOVDD _ (RP2350 only)a | Digital IO | 1.62 | 1.8 / 3.3 | 3.63 | V |
| QSPI IOVDD _ (RP2354 only) | Digital IO | 2.97 | 3.3 | 3.63 | V |
| DVDDb | Digital core | 1.05 | 1.1 | 1.16 | V |
| VREG VIN _ | Voltage regulator | 2.7 | 3.3 | 5.5 | V |
| VREG AVDD _ | Voltage regulator | 3.135 | 3.3 | 3.63 | V |
| USB OTP VDD _ _ | USB PHY & OTP | 3.135 | 3.3 | 3.63 | V |
| ADC AVDDc _ | ADC | 1.62 | 3.3 | 3.63 | V |

*Table 1441. Power*

14.9. Electrical specifications
1343

RP2350 Datasheet

a If IOVDD <2.5V, GPIO VOLTAGE_SELECT registers should be adjusted accordingly. See Section 6.1 for details.

b Short term transients should be within +/-100mV.

c ADC performance will be compromised at voltages below 2.97V

NOTE

RP2354 contains an internal 3.3V flash device, therefore QSPI_IOVDD must be 3.3V. Furthermore, if the QSPI pins are to

be used to connect to an additional flash or PSRAM device, then IOVDD must be 3.3V, as a GPIO is used as QSPI

chip select in this case.

## Embedded Images

![img_p1344_00.png](images/img_p1344_00.png)


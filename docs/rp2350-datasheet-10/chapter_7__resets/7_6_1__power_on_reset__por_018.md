---
source_pdf: rp2350-datasheet-10.pdf
repository: llm_database
chapter: Chapter 7. Resets
section: 7.6.1. Power-on reset (POR)
pages: 510-510
type: technical_spec
generated_at: 2026-03-01T12:20:14.494106+00:00
---

# 7.6.1. Power-on reset (POR)

RP2350 Datasheet

7.6.1. Power-on reset (POR)

The power-on reset block ensures the chip starts up cleanly when power is first applied. It accomplishes this by holding

the chip in reset until the digital core supply (DVDD) reaches a voltage high enough to reliably power the chip’s core logic.

The block holds its por_n output low until DVDD exceeds the power-on reset threshold (DVDDTH.POR) for a period greater

than the power-on reset assertion delay (tPOR.ASSERT). Once high, por_n remains high even if DVDD subsequently falls below

DVDDTH.POR. The behaviour of por_n when power is applied is shown in Figure 28, “A power-on reset cycle”.

![Page 510 figure](images/fig_p0510.png)

*Figure 28. A power-on reset cycle*

DVDDTH.POR is fixed at a nominal 0.957V, which should result in a threshold between 0.924V and 0.99V. The threshold

assumes a nominal DVDD of 1.1V at initial power-on, and por_n may never go high if a lower voltage is used. Once the chip

is out of reset, DVDD can be reduced without por_n going low.

7.6.1.1. Detailed specifications

| Parameter | Description | Min | Typ | Max | Units |
| --- | --- | --- | --- | --- | --- |
| DVDD TH.POR | power-on reset threshold | 0.924 | 0.957 | 0.99 | V |
| t POR.ASSERT | power-on reset assertion delay |  | 3 | 10 | μs |

*Table 538. Power-on*

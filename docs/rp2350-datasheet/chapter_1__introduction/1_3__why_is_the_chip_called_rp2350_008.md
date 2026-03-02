---
source_pdf: rp2350-datasheet.pdf
repository: llm_database
chapter: Chapter 1. Introduction
section: 1.3. Why is the chip called RP2350?
pages: 23-24
type: technical_spec
generated_at: 2026-03-02T14:11:45.471392+00:00
---

# 1.3. Why is the chip called RP2350?

*Figure 4. An explanation for the RP 2 3 5 0 name of the RP2350*

![Page 23 figure](images/fig_p0023.png)

floor(log2(nonvolatile / 128 kB))

floor(log2(RAM / 16 kB))

Type of core (e.g. Cortex-M33)

Number of cores

Raspberry Pi

The post-fix numeral on RP2350 comes from the following,

1. Number of processor cores

◦2 indicates a dual-core system

2. Loosely which type of processor

◦3 indicates Cortex-M33 or Hazard3

3. Internal memory capacity: 

◦5 indicates at least 25 × 16 kB = 512 kB

◦RP2350 has 520 kB of main system SRAM

4. Internal storage capacity: 
 (or 0 if no onboard nonvolatile storage)

◦RP2350 uses external flash

◦RP2354 has 24 × 128 kB = 2 MB of internal flash

## Embedded Images

![img_p0023_00.png](images/img_p0023_00.png)

![img_p0023_01.png](images/img_p0023_01.png)


---
source_pdf: rp2350-datasheet-4.pdf
repository: llm_database
chapter: Chapter 3. Processor subsystem
section: 3.5.1. Connecting to the SW-DP
pages: 86-86
type: technical_spec
generated_at: 2026-03-01T03:27:49.792955+00:00
---

# 3.5.1. Connecting to the SW-DP

![Page 86 figure](images/fig_p0086.png)

3.5.1. Connecting to the SW-DP

The SW-DP defaults to the Dormant state at power-up or assertion of the external reset (RUN) pin. A Dormant-to-SWD

sequence must be issued before beginning SWD operations. See the Arm Debug Interface specification, version 6, for

details of Dormant/SWD state switching: https://developer.arm.com/documentation/ihi0074/latest/

After a power-on, the following sequence can be used to connect to the SW-DP:

1. At least 8 × SWCLK cycles with SWDIO high.

3.5. Debug
85

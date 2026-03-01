---
source_pdf: rp2350-datasheet-4.pdf
repository: llm_database
chapter: Chapter 12. Peripherals
section: 12.7.1. Overview
pages: 1142-1142
type: technical_spec
generated_at: 2026-03-01T03:27:49.792955+00:00
---

# 12.7.1. Overview

![Page 1142 figure](images/fig_p1142.png)

Prerequisite knowledge required

This section requires knowledge of the USB protocol. If you aren’t yet familiar with the USB protocol, we recommend

the archive of the very useful USB Made Simple website. For formal definitions of the terminology used in this

section, see the USB 2.0 Specification.

RP2350 contains a USB 2.0 controller that can operate as either:

• a Full Speed (FS) device (12 Mb/s)
• a host that can communicate with both Low Speed (LS) (1.5 Mb/s) and Full Speed devices, including multiple

downstream devices connected to a USB hub

There is an integrated USB 1.1 PHY which interfaces the USB controller with the DP and DM pins of the chip. You may use

this as 3.3 V GPIO when the USB controller is not in use.

12.7. USB
1141

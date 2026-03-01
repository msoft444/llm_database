---
source_pdf: rp2350-datasheet-4.pdf
repository: llm_database
chapter: Chapter 1. Introduction
section: 1.1. The chip
pages: 15-15
type: technical_spec
generated_at: 2026-03-01T03:27:49.792955+00:00
---

# 1.1. The chip

RP2350 Datasheet

1.1. The chip

Dual Cortex-M33 or Hazard3 processors access RP2350’s memory and peripherals via AHB and APB bus fabric.

![Page 15 figure](images/fig_p0015.png)

Figure 1. A system

overview of the

RP2350 chip

Code may execute directly from external memory through a dedicated QSPI memory interface in the execute-in-place

subsystem (XIP). The cache improves XIP performance significantly. Both flash and RAM can attach via this interface.

Debug is available via the SWD interface. This allows an external host to load, run, halt and inspect software running on

the system, or configure the execution trace output.

Internal SRAM can contain code or data. It is addressed as a single 520 kB region, but physically partitioned into 10

banks to allow simultaneous parallel access from different managers. All SRAM supports single-cycle access.

A high-bandwidth system DMA offloads repetitive data transfer tasks from the processors.

GPIO pins can be driven directly via single-cycle IO (SIO), or from a variety of dedicated logic functions such as the

hardware SPI, I2C, UART and PWM. Programmable IO controllers (PIO) can provide a wider variety of IO functions, or

supplement the number of fixed-function peripherals.

A USB controller with embedded PHY provides FS/LS Host or Device connectivity under software control.

Four or eight ADC inputs (depending on package size) are shared with GPIO pins.

Two PLLs provide a fixed 48 MHz clock for USB or ADC, and a flexible system clock up to 150 MHz. A crystal oscillator

provides a precise reference for the PLLs.

An internal voltage regulator supplies the core voltage, so you need generally only supply the IO voltage. It operates as a

1.1. The chip
14

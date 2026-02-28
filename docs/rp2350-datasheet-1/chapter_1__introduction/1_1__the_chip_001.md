---
source_pdf: rp2350-datasheet-1.pdf
repository: llm_database
chapter: Chapter 1. Introduction
section: 1.1. The chip
pages: 15-15
type: technical_spec
generated_at: 2026-02-28T17:22:56.644477+00:00
---

# 1.1. The chip

1.1. The chip
Dual Cortex-M33 or Hazard3 processors access RP2350’s memory and peripherals via AHB and APB bus fabric.
IOs
PIO
Memory
Crystal
Clock
generation
Processor subsystem
Peripherals
Bus Fabric
Internal 
oscillator
USB
Proc0
ROM
SRAM
DMA
Core Supply Regulator
(Switcher and low 
power LDO)
XIP / 
Cache
SPI ×2
PWM
UART ×2
Timer
AON Timer
I2C ×2
ADC & TS
Reset control
Power control
Sysctrl
Sysinfo
Watchdog
Security
HSTX
PLL
PLL
Interrupts
SWD
QSPI
RP2350
SRAM
SRAM
SRAM
SRAM
OTP
SRAM
SRAM
SRAM
SRAM
SRAM
Proc1
PIO0 PIO1 PIO2
SIO
GPIO
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
RP2350 Datasheet
1.1. The chip
14


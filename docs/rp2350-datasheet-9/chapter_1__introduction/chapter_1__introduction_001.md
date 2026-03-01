---
source_pdf: rp2350-datasheet-9.pdf
repository: llm_database
chapter: Chapter 1. Introduction
section: Chapter 1. Introduction
pages: 14-14
type: technical_spec
generated_at: 2026-03-01T11:44:31.548839+00:00
---

# Chapter 1. Introduction

RP2350 Datasheet

Chapter 1. Introduction

RP2350 is a new family of microcontrollers from Raspberry Pi that offers major enhancements over RP2040. Key

features include:

• Dual Cortex-M33 or Hazard3 processors at 150 MHz
• 520 kB on-chip SRAM, in 10 independent banks
• 8 kB of one-time-programmable storage (OTP)
• Up to 16 MB of external QSPI flash or PSRAM through dedicated QSPI bus
• Additional 16 MB flash or PSRAM through optional second chip-select
• On-chip switched-mode power supply to generate core voltage
• Optional low-quiescent-current LDO mode for sleep states
• 2 × on-chip PLLs for internal or external clock generation
• GPIOs are 5 V-tolerant (powered) and 3.3 V-failsafe (unpowered)
• Security features:

◦Optional boot signing, enforced by on-chip mask ROM, with key fingerprint in OTP

◦Protected OTP storage for optional boot decryption key

◦Global bus filtering based on Arm or RISC-V security/privilege levels

◦Peripherals, GPIOs, and DMA channels individually assignable to security domains

◦Hardware mitigations for fault injection attacks

◦Hardware SHA-256 accelerator
• Peripherals:

◦2 × UARTs

◦2 × SPI controllers

◦2 × I2C controllers

◦24 × PWM channels

◦USB 1.1 controller and PHY, with host and device support

◦12 × PIO state machines

◦1 × HSTX peripheral

Table 1 shows the RP2350 family of devices, including options for QFN-80 (10 × 10 mm) and QFN-60 (7 × 7 mm)

packages, with and without flash-in-package.

| Product | Package | Internal Flash | GPIO | Analogue Inputs |
| --- | --- | --- | --- | --- |
| RP2350A | QFN-60 | None | 30 | 4 |
| RP2350B | QFN-80 | None | 48 | 8 |
| RP2354A | QFN-60 | 2 MB | 30 | 4 |
| RP2354B | QFN-80 | 2 MB | 48 | 8 |

*Table 1. RP2350 Chapter 1. Introduction 13*

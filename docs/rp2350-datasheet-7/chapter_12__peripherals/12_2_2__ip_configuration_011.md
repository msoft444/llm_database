---
source_pdf: rp2350-datasheet-7.pdf
repository: llm_database
chapter: Chapter 12. Peripherals
section: 12.2.2. IP configuration
pages: 985-986
type: technical_spec
generated_at: 2026-03-01T06:07:53.318745+00:00
---

# 12.2.2. IP configuration

12.2.2. IP configuration

I2C configuration details (each instance is fully independent):

• 32-bit APB access
• Supports Standard mode, Fast mode or Fast mode plus (not High speed)
• Default slave address of 0x055
• Master or Slave mode
• Master by default (Slave mode disabled at reset)

12.2. I2C
984

RP2350 Datasheet

• 10-bit addressing supported in master mode (7-bit by default)
• 16 entry transmit buffer
• 16 entry receive buffer
• Allows restart conditions when a master (can be disabled for legacy device support)
• Configurable timing to adjust TsuDAT/ThDAT
• General calls responded to on reset
• Interface to DMA
• Single interrupt output
• Configurable timing to adjust clock frequency
• Spike suppression (default 7 clk_sys cycles)
• Can NACK after data received by Slave
• Hold transfer when TX FIFO empty
• Hold bus until space available in RX FIFO
• Restart detect interrupt in Slave mode
• Optional blocking Master commands (not enabled by default)

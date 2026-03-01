---
source_pdf: rp2350-datasheet-10.pdf
repository: llm_database
chapter: Chapter 3. Processor subsystem
section: 3.8. Hazard3 processor
pages: 234-234
type: technical_spec
generated_at: 2026-03-01T12:20:14.494106+00:00
---

# 3.8. Hazard3 processor

RP2350 Datasheet

3.8. Hazard3 processor

Hazard3 is a low-area, high-performance RISC-V processor with a 3-stage in-order pipeline. RP2350 configures the

following standard RISC-V extensions:

• RV32I: 32-bit base instruction set
• M: Integer multiply/divide/modulo instructions
• A: Atomic memory operations
• C: Compressed 16-bit instructions (equivalently spelled Zca)
• Zba: Address generation instructions
• Zbb: Basic bit manipulation instructions
• Zbs: Single-bit manipulation instructions
• Zbkb: Basic bit manipulation for scalar cryptography
• Zcb: Basic additional compressed instructions
• Zcmp: Push/pop and double-move compressed instructions
• Zicsr: CSR access instructions
• Debug, Machine and User execution modes
• Physical Memory Protection unit (PMP) with eight regions, 32-byte granule, NAPOT
• External debug support with four instruction address triggers

Additionally, RP2350 enables the following Hazard3 custom extensions:

• Xh3power: Power management instructions and CSRs
• Xh3bextm: Bit-extract-multiple instruction (used in bootrom)
• Xh3irq: Local interrupt controller with nested, prioritised IRQ support
• Xh3pmpm: Unlocked M-mode PMP regions

Hazard3 Source Code

All hardware source files for Hazard3 are available under Apache 2.0 licensing at:

github.com/wren6991/hazard3

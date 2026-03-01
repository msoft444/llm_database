---
source_pdf: rp2350-datasheet-10.pdf
repository: llm_database
chapter: Chapter 3. Processor subsystem
section: 3.2. Interrupts
pages: 83-84
type: technical_spec
generated_at: 2026-03-01T12:20:14.494106+00:00
---

# 3.2. Interrupts

3.2. Interrupts

Each core is equipped with an internal interrupt controller, with 52 interrupt inputs. For the most part each core has

exactly the same interrupts routed to it, though there are some exceptions, referred to as core-local interrupts, where

there is an individual per-core interrupt source mapped to the same interrupt number on each core:

• Cross-core FIFO interrupts: SIO_IRQ_FIFO and SIO_IRQ_FIFO_NS (Section 3.1.5)
• Cross-core doorbell interrupts: SIO_IRQ_BELL and SIO_IRQ_BELL_NS (Section 3.1.6)
• RISC-V platform timer (also usable by Arm cores): SIO_IRQ_MTIMECMP (Section 3.1.8)
• GPIO interrupts: IO_IRQ_BANK0, IRQ_IO_BANK0_NS, IO_IRQ_QSPI, IO_IRQ_QSPI_NS (Section 9.5)

The remaining interrupt inputs have the same interrupt source mirrored identically on both cores. Non-core-local

interrupts should only be enabled in the interrupt controller of a single core at a time, and will be serviced by the core

whose interrupt controller they are enabled in.

| IRQ | Interrupt Source | IRQ | Interrupt Source | IRQ | Interrupt Source | IRQ | Interrupt Source | IRQ | Interrupt Source |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 0 | TIMER0 IRQ 0 _ _ | 11 | DMA IRQ 1 _ _ | 22 | IO IRQ BANK0 NS _ _ _ | 33 | UART0 IRQ _ | 44 | POWMAN IRQ POW _ _ |
| 1 | TIMER0 IRQ 1 _ _ | 12 | DMA IRQ 2 _ _ | 23 | IO IRQ QSPI _ _ | 34 | UART1 IRQ _ | 45 | POWMAN IRQ TIMER _ _ |
| 2 | TIMER0 IRQ 2 _ _ | 13 | DMA IRQ 3 _ _ | 24 | IO IRQ QSPI NS _ _ _ | 35 | ADC IRQ FIFO _ _ | 46 | SPAREIRQ IRQ 0 _ _ |
| 3 | TIMER0 IRQ 3 _ _ | 14 | USBCTRL IRQ _ | 25 | SIO IRQ FIFO _ _ | 36 | I2C0 IRQ _ | 47 | SPAREIRQ IRQ 1 _ _ |
| 4 | TIMER1 IRQ 0 _ _ | 15 | PIO0 IRQ 0 _ _ | 26 | SIO IRQ BELL _ _ | 37 | I2C1 IRQ _ | 48 | SPAREIRQ IRQ 2 _ _ |
| 5 | TIMER1 IRQ 1 _ _ | 16 | PIO0 IRQ 1 _ _ | 27 | SIO IRQ FIFO NS _ _ _ | 38 | OTP IRQ _ | 49 | SPAREIRQ IRQ 3 _ _ |
| 6 | TIMER1 IRQ 2 _ _ | 17 | PIO1 IRQ 0 _ _ | 28 | SIO IRQ BELL NS _ _ _ | 39 | TRNG IRQ _ | 50 | SPAREIRQ IRQ 4 _ _ |
| 7 | TIMER1 IRQ 3 _ _ | 18 | PIO1 IRQ 1 _ _ | 29 | SIO IRQ MTIMECMP _ _ | 40 | PROC0 IRQ CTI _ _ | 51 | SPAREIRQ IRQ 5 _ _ |
| 8 | PWM IRQ WRAP 0 _ _ _ | 19 | PIO2 IRQ 0 _ _ | 30 | CLOCKS IRQ _ | 41 | PROC1 IRQ CTI _ _ |  |  |
| 9 | PWM IRQ WRAP 1 _ _ _ | 20 | PIO2 IRQ 1 _ _ | 31 | SPI0 IRQ _ | 42 | PLL SYS IRQ _ _ |  |  |
| 10 | DMA IRQ 0 _ _ | 21 | IO IRQ BANK0 _ _ | 32 | SPI1 IRQ _ | 43 | PLL USB IRQ _ _ |  |  |

*Table 95. System-level interrupt numbering. All interrupts are routed to both processors.*

3.2. Interrupts
82

RP2350 Datasheet

On RP2350, only the lower 46 IRQ signals are connected to system-level interrupt sources, and IRQs 46 to 51 are

hardwired to zero (never firing). These six spare interrupts, referred to as SPAREIRQ_IRQ_0 through SPAREIRQ_IRQ_5 in the

table, are deliberately reserved for the cores to interrupt themselves (via the Arm NVIC_ISPR0 registers or the Hazard3

MEIFA CSR), for example, when an interrupt handler wants to schedule a "bottom half" handler for work that must be

done after exiting the interrupt handler, but before returning to the code running in the foreground.

Nested interrupts are supported in hardware: a lower-priority interrupt can be pre-empted by a higher-priority interrupt or

fault, and will resume once the higher-priority handler returns. The pre-emption priority order is determined by the

interrupt priority registers starting from NVIC_IPR0 (Cortex-M33) or the MEIPRA interrupt priority array CSR (Hazard3).

When there is a choice of multiple interrupts to be entered at the same dynamic priority, the interrupt with the lowest

IRQ number is chosen as a tie-breaker. The system-level IRQ numbering has been chosen to generally put higher-priority

interrupts at lower IRQ numbers for this reason, though the true priority is often dependent on the specific application.

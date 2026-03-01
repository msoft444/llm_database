---
source_pdf: rp2350-datasheet-3.pdf
repository: llm_database
chapter: Chapter 12. Peripherals
section: 12.6.8. Additional features
pages: 1108-1108
type: technical_spec
generated_at: 2026-03-01T02:30:35.833805+00:00
---

# 12.6.8. Additional features

RP2350 Datasheet

12.6.8. Additional features

12.6.8.1. Pacing timers

These allow transfer of data roughly once every n clk_sys clocks instead of using external peripheral DREQ to trigger

transfers. A fractional (X/Y) divider is used, and will generate a maximum of 1 request per clk_sys cycle.

There are 4 timers available in RP2350. Each DMA channel is able to select any of these in CTRL.TREQ_SEL. There is one

register used to configure the pacing coefficients for each timer, TIMER0 through TIMER3.

Each timer’s security level is defined by a register field in SECCFG_MISC. This defines the minimum bus security level

required to configure that timer (lower levels will get a bus fault), and the minimum channel security level required to

observe that timer’s TREQ.

12.6.8.2. CRC calculation

The DMA can watch data from a given channel passing through the data FIFO, and calculate checksums based on this

data. This a purely passive affair: the data is not altered by this hardware, only observed.

The feature is controlled via the SNIFF_CTRL and SNIFF_DATA registers, and can be enabled/disabled per DMA transfer via

the CTRL.SNIFF_EN field.

As this hardware cannot place back-pressure on the FIFO, it must keep up with the DMA’s maximum transfer rate of 32

bits per clock.

The supported checksums are:

• CRC-32, MSB-first and LSB-first
• CRC-16-CCITT, MSB-first and LSB-first
• Simple summation (add to 32-bit accumulator)
• Even parity

The result register is both readable and writable, so that the initial seed value can be set.

Bit/byte manipulations are available on the result, which can aid specific use cases:

• Bit inversion
• Bit reversal
• Byte swap

These manipulations do not affect the CRC calculation, just how the data is presented in the result register.

The sniffer’s security level is configured by the SECCFG_MISC.SNIFF_S and SECCFG_MISC.SNIFF_P bits. This

determines the minimum bus security level required to access the sniffer’s control registers, as well as the maximum

channel security level that the sniffer can observe.

12.6.8.3. Channel abort

It is possible for a channel to get into an irrecoverable state. If commanded to transfer more data than a peripheral will

ever request, the channel will never complete. Clearing the CTRL.EN bit pauses the channel, but does not solve the

problem. This should not occur under normal circumstances, but it is important that there is a mechanism to recover

without simply hard-resetting the entire DMA block.

In such a situation, use the CHAN_ABORT register to force the channel to complete early. There is one bit for each

channel. Writing a 1 to the corresponding bit terminates the channel. This clears the transfer counter and forces the

channel into an inactive state.

12.6. DMA
1107

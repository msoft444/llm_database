---
source_pdf: rp2350-datasheet-1.pdf
repository: llm_database
chapter: Chapter 12. Peripherals
section: 12.6.1. Changes from RP2040
pages: 1096-1096
type: technical_spec
generated_at: 2026-02-28T17:22:56.644477+00:00
---

# 12.6.1. Changes from RP2040

Control/Status 
Registers
Read Address FIFO
Write Address FIFO
Address Generator
AHB5 
Read Manager
From 
System
Transfer Data FIFO
AHB5 
Write Manager
To 
System
AHB5 
Subordinate 
Interface
Figure 122. DMA
Architecture Overview.
The read manager can
read data from some
address every clock
cycle. Likewise, the
write manager can
write to another
address. The address
generator produces
matched pairs of read
and write addresses,
which the managers
consume through the
address FIFOs. The
DMA can run up to 16
transfer sequences
simultaneously,
supervised by
software via the
control and status
registers.
The DMA can perform one read access and one write access, up to 32 bits in size, every clock cycle. There are 16
independent channels, each of which supervises a sequence of bus transfers in one of the following scenarios:
Memory-to-peripheral
a peripheral signals the DMA when it needs more data to transmit. The DMA reads data from an array in RAM or
flash, and writes to the peripheral’s data FIFO.
Peripheral-to-memory
a peripheral signals the DMA when it has received data. The DMA reads this data from the peripheral’s data FIFO,
and writes it to an array in RAM.
Memory-to-memory
the DMA transfers data between two buffers in RAM, as fast as possible.
Each channel has its own control and status registers (CSRs) that software can use to program and monitor the
channel’s progress. When multiple channels are active at the same time, the DMA shares bandwidth evenly between the
channels, with round-robin over all channels that are currently requesting data transfers.
The transfer size can be either 32, 16, or 8 bits. This is configured once for each channel: source transfer size and
destination transfer size are the same. The DMA performs byte lane replication on narrow writes, so byte data is
available in all 4 bytes of the databus, and halfword data in both halfwords.
Channels can be combined in varied ways for more sophisticated behaviour and greater autonomy. For example, one
channel can configure another, loading configuration data from a sequence of control blocks in memory, and the
second can then call back to the first via the CHAIN_TO option when it needs to be reconfigured.
Making the DMA more autonomous means that much less processor supervision is required: overall this allows the
system to do more at once, or to dissipate less power.
12.6.1. Changes from RP2040
The following new features have been added:
• Increased the number of DMA channels from 12 to 16.
• Increased the number of shared IRQ outputs from 2 to 4.
• Channels can be assigned to security domains using SECCFG_CH0 through SECCFG_CH15.
• The DMA now filters bus accesses using the built-in memory protection unit (Section 12.6.6.3).
• Interrupts can be assigned to security domains using SECCFG_IRQ0 through SECCFG_IRQ3.
• Pacing timers and the CRC sniffer can be assigned to security domains using the SECCFG_MISC register.
• The four most-significant bits of TRANS_COUNT (CH0_TRANS_COUNT) are redefined as the MODE field, which defines
what happens when TRANS_COUNT reaches zero:
RP2350 Datasheet
12.6. DMA
1095


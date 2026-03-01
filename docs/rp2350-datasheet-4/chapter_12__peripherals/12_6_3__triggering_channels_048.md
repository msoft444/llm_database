---
source_pdf: rp2350-datasheet-4.pdf
repository: llm_database
chapter: Chapter 12. Peripherals
section: 12.6.3. Triggering channels
pages: 1099-1100
type: technical_spec
generated_at: 2026-03-01T03:27:49.792955+00:00
---

# 12.6.3. Triggering channels

12.6.3. Triggering channels

After a channel has been correctly configured, you must trigger it. This instructs the channel to begin scheduling bus

accesses, either paced by a peripheral data request signal (DREQ) or as fast as possible. The following events can

trigger a channel:

• A write to a channel trigger register.
• Completion of another channel whose CHAIN_TO points to this channel.
• A write to the MULTI_CHAN_TRIGGER register (can trigger multiple channels at once).

Each trigger mechanism covers different use cases. For example, trigger registers are simple and efficient when

12.6. DMA
1098

RP2350 Datasheet

configuring and starting a channel in an interrupt service routine because the channel is triggered by the last

configuration write. CHAIN_TO allows one channel to callback to another channel, which can then reconfigure the first

channel. MULTI_CHAN_TRIGGER allows software to simply start a channel without touching any of its configuration

registers.

When triggered, the channel sets its CTRL.BUSY flag to indicate it is actively scheduling transfers. This remains set until

the transfer count reaches zero, or the channel is aborted via the CHAN_ABORT register (Section 12.6.8.3).

When a channel is already running, indicated by BUSY = 1, it ignores additional triggers. A channel that is disabled (CTRL.EN

is clear) also ignores triggers.

12.6.3.1. Aliases and triggers

| Offset | +0x0 | +0x4 | +0x8 | +0xc (Trigger) |
| --- | --- | --- | --- | --- |
| 0x00 (Alias 0) | READ ADDR _ | WRITE ADDR _ | TRANS COUNT _ | CTRL TRIG _ |
| 0x10 (Alias 1) | CTRL | READ ADDR _ | WRITE ADDR _ | TRANS COUNT TRIG _ _ |
| 0x20 (Alias 2) | CTRL | TRANS COUNT _ | READ ADDR _ | WRITE ADDR TRIG _ _ |
| 0x30 (Alias 3) | CTRL | WRITE ADDR _ | TRANS COUNT _ | READ ADD TRIG _ _ |

Table 1145. Control

register aliases. Each

channel has four

control/status

registers. Each

register can be

accessed at multiple

different addresses. In

each naturally-aligned

group of four, all four

registers appear, in

different orders.

The four CSRs are aliased multiple times in memory. Each of the four aliases exposes the same four physical registers,

but in a different order. The final register in each alias (at offset +0xc, highlighted) is a trigger register. Writing to the

trigger register starts the channel.

Often, only alias 0 is used, and aliases 1 through 3 can be ignored. To configure and start the channel, write READ_ADDR,

WRITE_ADDR, TRANS_COUNT, and finally CTRL. Since CTRL is the trigger register in alias 0, this starts the channel.

The other aliases allow more compact control block lists when using one channel to configure another, and more

efficient reconfiguration and launch in interrupt handlers:

• Each CSR is a trigger register in one of the aliases:

◦When gathering fixed-size buffers into a peripheral, the DMA channel can be configured and launched by

writing only READ_ADDR_TRIG.

◦When scattering from a peripheral to fixed-size buffers, the channel can be configured and launched by

writing only WRITE_ADDR_TRIG.
• Useful combinations of registers appear as naturally-aligned tuples which contain a trigger register. In conjunction

with channel chaining and address wrapping, these implement compressed control block formats, e.g.:

◦(WRITE_ADDR, TRANS_COUNT_TRIG) for peripheral scatter operations

◦(TRANS_COUNT, READ_ADDR_TRIG) for peripheral gather operations, or calculating CRCs on a list of buffers

◦(READ_ADDR, WRITE_ADDR_TRIG) for manipulating fixed-size buffers in memory

Trigger registers do not start the channel if:

• The channel is disabled via CTRL.EN (if the trigger is CTRL, the just-written value of EN is used, not the value currently

in the CTRL register)
• The channel is already running
• The value 0 is written to the trigger register (useful for ending control block chains, see null triggers (Section

12.6.3.3))
• The bus access has a security level lower than the channel’s security level (Section 12.6.6.1)

12.6. DMA
1099

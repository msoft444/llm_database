---
source_pdf: rp2350-datasheet-3.pdf
repository: llm_database
chapter: Chapter 12. Peripherals
section: 12.6.3. Triggering channels
pages: 1099-1100
type: technical_spec
generated_at: 2026-03-01T02:30:35.833805+00:00
---

# 12.6.3. Triggering channels

RP2350 Datasheet

NOTE

The TRANS_COUNT is the number of transfers to be performed. The total number of bytes transferred is TRANS_COUNT

times the size of each transfer in bytes, given by CTRL.DATA_SIZE.

12.6.2.2.1. Count modes

The four most-significant bits of TRANS_COUNT contain the MODE field (CH0_TRANS_COUNT.MODE), which modifies the

counting behaviour of TRANS_COUNT. Mode 0x0 is the default: TRANS_COUNT decrements once for every bus transfer, and the

channel halts once TRANS_COUNT reaches zero and all in-flight transfers have finished. The value of 0x0 is chosen for

backward-compatibility with RP2040 software, which expects the TRANS_COUNT register to contain a 32-bit count rather

than a 4-bit mode and a 28-bit count. There are few use cases for a finite number of transfers greater than 228, which is

why the four most-significant bits have been reallocated for use with endless transfers.

Mode 0x1, TRIGGER_SELF, behaves the same as mode 0x0, except that rather than halting upon completion, the channel

immediately re-triggers itself. This is equivalent to a trigger performed by any other mechanism (Section 12.6.3):

TRANS_COUNT is reloaded, and the channel resumes from the current READ_ADDR and WRITE_ADDR addresses. A completion

interrupt is still raised (if CTRL.IRQ_QUIET is not set) and the specified CHAIN_TO operation is still performed. The main use

for this mode is streaming through SRAM ring buffers, where some action is required at regular intervals, for example

requesting the processor to refill an audio buffer once it is half-empty.

Mode 0xf, ENDLESS, disables the decrement of TRANS_COUNT. This means a channel will generally run indefinitely without

pause, though triggering a channel with a mode of 0xf and a count of 0x0 will result in the channel halting immediately.

All other values are reserved for future use and their effect is unspecified.

12.6.2.3. Control/Status

The CTRL register (CH0_CTRL_TRIG) has more, smaller fields than the other 3 registers. Among other things, CTRL is used

to:

• Configure the size of this channel’s data transfers through the DATA_SIZE field. Reads are always the same size as

writes.
• Configure if and how READ_ADDR and WRITE_ADDR increment after each read or write through the INCR_READ,

INCR_READ_REV, INCR_WRITE, INCR_WRITE_REV, RING_SEL, and RING_SIZE fields. Ring transfers are available, where one of the

address pointers wraps at some power-of-2 boundary.
• Select another channel (or none) to trigger when this channel completes through the CHAIN_TO field.
• Select a peripheral data request (DREQ) signal to pace this channel’s transfers, via the TREQ_SEL field.
• See when the channel is idle, using the BUSY flag.
• See if the channel has encountered a bus error in the READ_ERROR and WRITE_ERROR flags, or the combined error status

in the AHB_ERROR flag.

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

![Page 1100 figure](images/fig_p1100.png)

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

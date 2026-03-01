---
source_pdf: rp2350-datasheet-10.pdf
repository: llm_database
chapter: Chapter 12. Peripherals
section: 12.6.1. Changes from RP2040
pages: 1096-1097
type: technical_spec
generated_at: 2026-03-01T12:20:14.494106+00:00
---

# 12.6.1. Changes from RP2040

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

12.6. DMA
1095

RP2350 Datasheet

◦This backward-incompatible change reduces the maximum transfers in one sequence from 232-1 to 228-1.

◦Mode 0x0 has the same behaviour as RP2040, so there is no need to modify software that performs less than

256 million transfers at a time.

◦Mode 0x1, "trigger self", allows a channel to automatically restart itself after finishing a transfer sequence, in

addition to the usual end-of-sequence actions like raising an interrupt or triggering other channels. This can

be used for example to get periodic interrupts from streaming ring buffer transfers.

◦Mode 0xf, "endless", allows a channel to run forever: TRANS_COUNT does not decrement.
• New CH0_CTRL_TRIG.INCR_READ_REV and CH0_CTRL_TRIG.INCR_WRITE_REV fields allow addresses to

decrement rather than increment, or to increment by two.

◦Some existing fields in the CTRL registers, such as CH0_CTRL_TRIG.BUSY, have moved to accommodate the

new fields.

Some existing behaviour has been refined:

• The logic that adjusts values read from WRITE_ADDR and READ_ADDR according to the number of in-flight transfers is

disabled for address-wrapping and non-incrementing transfers (erratum RP2040-E12).
• You can now poll the ABORT register to wait for completion of an aborted channel (erratum RP2040-E13).
• DMA completion actions such as CHAIN_TO are now strictly ordered against the last write completion, so a CHAIN_TO

on a channel whose registers you write to is a well-defined operation.

◦This enables the use of control blocks that don’t include one of the four trigger register aliases.

◦Previously, a channel was considered to complete on the first cycle of its last write’s data phase. Now, a

channel is considered to complete on the last cycle of its last write’s data phase. This is usually the same

cycle, but it can be later when the DMA encounters a write data-phase bus stall.
• Previously, the DMA’s internal arbitration logic inserted an idle cycle after completing a round of active high-priority

channels (CH0_CTRL_TRIG.HIGH_PRIORITY), even if there were no active low-priority requests. This reduced DMA

throughput when lightly loaded. This idle cycle has been removed, eliminating lost throughput.
• IRQ assertion latency has been reduced by one cycle.

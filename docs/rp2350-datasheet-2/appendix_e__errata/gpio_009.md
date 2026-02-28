---
source_pdf: rp2350-datasheet-2.pdf
repository: llm_database
chapter: Appendix E: Errata
section: GPIO
pages: 1366-1366
type: technical_spec
generated_at: 2026-02-28T17:43:10.557178+00:00
---

# GPIO

![Page 1366 figure](images/fig_p1366.png)

RP2350 Datasheet

| Reference | RP2350-E5 |
| --- | --- |
| Summary | Interactions between CHAIN TO and ABORT of active channels
_ |
| Affects | RP2350 A2, RP2350 A3, RP2350 A4 |
| Description | The CHAN_ABORT register commands a DMA channel to stop issuing transfers, and to clear its BUSY flag
once in-flight transfers have completed. This was originally intended for recovering channels that are
stuck with their DREQ low. An ABORT is initiated by writing a bitmap of aborted channels to CHAN_ABORT.
Bits remain set until each channel comes to rest.
This erratum is a compound of two behaviours: first, aborting a channel will cause its CHAIN TO to fire, if
_
and only if the aborted channel is the last channel to have completed a write transfer. Second, a channel
undergoing an ABORT is susceptible to be re-triggered on the last cycle before the ABORT register clears,
because the channel is both inactive and enabled on this cycle, and the ABORT itself doesn’t inhibit
triggering. However, since the ABORT is still in effect, the transfer count is held at zero. On the cycle after
the ABORT finishes, the channel completes because its transfer counter is zero. This causes the channel’s
IRQ and CHAIN TO to fire on the cycle after the ABORT completes.
_
These two behaviours are problematic when aborting multiple channels that chain to one another, since
they may cause the channels to immediately restart post-abort. |
| Workaround | Before aborting an active channel, clear the EN bit (CH0_CTRL_TRIG.EN) of both the aborted channel and
any channel it chains to. This ensures the channel isn’t susceptible to re-triggering. |
| Fixed by | Documentation |

RP2350-E8

| Reference | RP2350-E8 |
| --- | --- |
| Summary | CHAIN TO might not fire for zero-length transfers
_ |
| Affects | RP2350 A2, RP2350 A3, RP2350 A4 |
| Description | The CTRL.CHAIN TO field configures a channel to start another channel once it completes its programmed
_
sequence of transfers. The CHAIN TO takes place on the cycle where the channel’s last write completes,
_
and the chainee becomes active on the next cycle.
The hardware implementation assumes that CHAIN TO always happens as a result of a write completion.
_
This isn’t the case when a channel is triggered with a transfer count of zero; in this case the channel
completes on the cycle immediately after the trigger without performing any bus accesses.
A CHAIN TO from a channel started with a transfer count of zero will fire if and only if that channel is the
_
last channel to have completed a write transfer. This is true only when the channel in question has
previously performed a non-zero-length sequence of transfers, and no other channel has completed a
write since. |
| Workaround | Don’t use CHAIN TO in conjunction with zero-length transfers. Avoid zero-length transfers in the middle of
_
control block lists, and replace them with dummy transfers if possible. |
| Fixed by | Documentation |

GPIO

GPIO
1365

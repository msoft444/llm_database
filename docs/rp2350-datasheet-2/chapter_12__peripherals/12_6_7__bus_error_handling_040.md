---
source_pdf: rp2350-datasheet-2.pdf
repository: llm_database
chapter: Chapter 12. Peripherals
section: 12.6.7. Bus error handling
pages: 1106-1107
type: technical_spec
generated_at: 2026-02-28T17:43:10.557178+00:00
---

# 12.6.7. Bus error handling

RP2350 Datasheet

Each of the eight MPU regions is configured with a base address, MPU_BAR0 through MPU_BAR7 for each region, and a

limit address, MPU_LAR0 through MPU_LAR7.

MPU regions have a granularity of 32 bytes, so the base/limit addresses are configured by the 27 most-significant bits

of each BAR/LAR register (bits 31:5). Addresses match MPU regions when the 27 most-significant bits of the address are

greater than or equal to the BAR address bits, and less than or equal to the LAR address bits. For example, when

MPU_BAR0 and MPU_LAR0 both have the value 0x10000000, MPU region 0 matches on a 32-byte region extending from

byte address 0x10000000 to 0x1000001f (inclusive). Regions can be enabled or disabled using the LAR.EN bits — if a region is

disabled, it matches no addresses.

The minimum security level required to access each region is defined by the S and P bits in the LSBs of that region’s LAR

register. When an address matches multiple regions, the lowest-numbered region applies. This matches the tie-break

rules for the RISC-V PMP, but is different from the Arm SAU tie-break rules, so care must be taken when mirroring SAU

mappings with overlapping regions. When none of the MPU regions are matched, the security level is defined by the

global MPU_CTRL.S and MPU_CTRL.P bits.

The MPU configuration registers (MPU_CTRL, MPU_BAR0 through MPU_BAR7 and MPU_LAR0 through MPU_LAR7) do

not permit unprivileged access. Bus accesses at the SU and NSU security levels will return a bus fault and have no other

effect.

The MPU registers are also mostly read-only to NSP accesses, with the sole exception being the region P bits which are

NSP-writable if and only if the corresponding region’s S bit is clear. This delegates to Privileged, Non-secure software

the decision of whether Non-secure regions are NSU-accessible.

12.6.7. Bus error handling

A bus error is an error condition flagged to one of the DMA’s manager ports in response to an attempted read or write

transfer, indicating the transfer was rejected for one of the following reasons:

• The DMA MPU forbids access to this address at the originating channel’s security level (Section 12.6.6.3).
• The bus fabric failed to decode the address; the address did not match any known memory location (for example

SIO is not visible from the DMA bus ports as it is tightly coupled to the processors).
• ACCESSCTRL forbids access to the addressed region at the originating channel’s privilege level (Section 10.6.2).
• ACCESSCTRL forbids DMA access to the addressed region, irrespective of privilege.
• The APB bridge returned a timeout fault for a transfer exceeding 65535 cycles (e.g. accessed ADC whilst clk_adc

was stopped).
• The downstream bus port returned an error response for any other device-specific reason, e.g. attempting to

access configuration registers for a DMA channel with higher security level (Section 12.6.6.1).

12.6.7.1. Response to bus errors

Upon encountering a bus error, the DMA halts the offending channel and reports the error through the channel’s

CH0_CTRL_TRIG.READ_ERROR and WRITE_ERROR flags. The channel stops scheduling bus accesses.

Bus errors are exceptional events which usually indicate misconfiguration of the DMA or some other system hardware.

Therefore the DMA refuses to restart the offending channel until its error status is cleared by writing 1 to the relevant

error flag. Other channels are not affected, and continue their transfer sequences uninterrupted.

A channel which encounters a bus error does not CHAIN_TO other channels.

Bus errors always cause the channel’s interrupt request to be asserted. Whether or not this causes a system-level IRQ

depends on the channel masks configured in interrupt enable registers INTE0 through INTE3.

12.6. DMA
1105

RP2350 Datasheet

12.6.7.2. Recovery after bus errors

If an error is reported through READ_ERR/WRITE_ERR then, before restarting the channel, software must:

1. Poll for a low BUSY status to ensure that all in-flight transfers for this channel have been flushed from the DMA’s bus

pipeline.

2. Clear the error flags by writing 1 to each flag.

Generally the BUSY flag will already be low long before the processor enters its interrupt handler and checks the error

status, but it is possible for these events to overlap when the DMA is accessing a slow device such as XIP with a high

SCK divisor and processors are executing from SRAM.

READ_ADDR and WRITE_ADDR contain the approximate address where the bus error was encountered. This can be useful for

the programmer to understand why the bus error occurred, and fix the software to avoid it in future.

Since the DMA performs reads and writes in parallel, it is possible for a channel to encounter both a read and write error

simultaneously, and in this case the DMA sets both READ_ERR and WRITE_ERR. You must clear both.

12.6.7.3. Halt timing

The DMA halts the channel as soon as possible following a bus error. This suppresses future reads and writes. Because

the request to access the bus is masked, the bus access has no side effects on the system. The timing relationships are

not straightforward due to the DMA’s pipelining and buffering. The DMA provides the following ordering guarantees

between transfers originating from one channel:

• Read error → read suppression: Any reads scheduled to occur after a faulting read will be suppressed, but can still

increment READ_ADDR up to two times total
• Write error → write suppression: Any writes scheduled to occur after a faulting write will be suppressed, but can

still increment WRITE_ADDR up to four times total
• Read error → write suppression:

◦Any write paired with a faulting read will be suppressed, but will increment WRITE_ADDR

◦Any write following the first write paired with a faulting read will be suppressed, but can increment WRITE_ADDR

up to three times total

◦Up to three writes immediately preceding the first write paired with a faulting read can be suppressed, but will

increment WRITE_ADDR
• Write error → read suppression:

◦Reads paired with writes before the first faulting write will not be suppressed, and will increment READ_ADDR.

◦Up to two read transfers paired with writes after the first faulting write can be suppressed, and can increment

READ_ADDR

"Paired with" in the above paragraph refers to the write access which writes data originating from a particular read

transfer, or vice versa. The DMA always schedules read and write accesses in matched pairs.

Slight variability in halt behaviour is due to the buffering of in-flight transfers, and the parallel operation of the read and

write bus ports. The values of READ_ADDR/WRITE_ADDR following a bus error can be slightly beyond the address that

experienced the first error, but the difference is bounded, and usually this is still sufficient to diagnose the reason for the

fault. Additionally, READ_ADDR and WRITE_ADDR are guaranteed to over-increment by the same amount, since reads and

writes are always scheduled in pairs.

In addition to the increments mentioned above, READ_ADDR/WRITE_ADDR always point to the next address to be written, so

always point slightly past the faulting address if address increment is enabled.

12.6. DMA
1106

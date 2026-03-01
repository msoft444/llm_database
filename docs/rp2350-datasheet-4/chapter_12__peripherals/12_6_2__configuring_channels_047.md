---
source_pdf: rp2350-datasheet-4.pdf
repository: llm_database
chapter: Chapter 12. Peripherals
section: 12.6.2. Configuring channels
pages: 1097-1098
type: technical_spec
generated_at: 2026-03-01T03:27:49.792955+00:00
---

# 12.6.2. Configuring channels

12.6.2. Configuring channels

Each channel has four control/status registers:

• READ_ADDR (CH0_READ_ADDR) is the address of the next memory location to read.
• WRITE_ADDR (CH0_WRITE_ADDR) is the address of the next memory location to write.
• TRANS_COUNT (CH0_TRANS_COUNT) shows the number of transfers remaining in the current transfer sequence and

programs the number of transfers in the next transfer sequence (see Section 12.6.2.2).
• CTRL (CH0_CTRL_TRIG) configures all other aspects of the channel’s behaviour, enables/disables the channel, and

provides completion status.

To directly instruct the DMA channel to perform a data transfer, software writes to these four registers, and then

triggers the channel (Section 12.6.3). To make the DMA more autonomous, you can also program one DMA channel to

write to another channel’s configuration registers, queueing up many transfer sequences in advance.

All four are live registers; they update their status continuously as the channel progresses.

12.6.2.1. Read and write addresses

READ_ADDR and WRITE_ADDR contain the address the channel will next read from, and write to, respectively. These registers

update automatically after each read/write access, incrementing to the next read/write address as required. The size of

the increment varies according to:

12.6. DMA
1096

RP2350 Datasheet

• the transfer size: 1, 2 or 4 byte bus accesses as per CH0_CTRL_TRIG.DATA_SIZE
• the increment enable for each address register: CH0_CTRL_TRIG.INCR_READ and CH0_CTRL_TRIG.INCR_WRITE
• the increment direction: CH0_CTRL_TRIG.INCR_READ_REV and CH0_CTRL_TRIG.INCR_WRITE_REV

Software should generally program these registers with new start addresses each time a new transfer sequence starts.

If READ_ADDR and WRITE_ADDR are not reprogrammed, the DMA will use the current values as start addresses for the next

transfer. For example:

• If the address does not increment (e.g. it is the address of a peripheral FIFO), and the next transfer sequence is

to/from that same address, there is no need to write to the register again.
• When transferring to/from a consecutive series of buffers in memory (e.g. scattering and gathering), an address

register will already have incremented to the start of the next buffer at the completion of a transfer.

By not programming all four CSRs for each transfer sequence, software can use shorter interrupt handlers, and more

compact control block formats when used with channel chaining (see register aliases in Section 12.6.3.1, chaining in

Section 12.6.3.2).

12.6.2.1.1. Address alignment

READ_ADDR and WRITE_ADDR must be aligned to the transfer size, specified in CH0_CTRL_TRIG.DATA_SIZE. For 32-bit

transfers, the address must be a multiple of four, and for 16-bit transfers, the address must be a multiple of two.

Software is responsible for correctly aligning addresses written to READ_ADDR and WRITE_ADDR: the DMA does not enforce

alignment.

If software initially writes a correctly aligned address, the address will remain correctly aligned throughout the transfer

sequence, because the DMA always increments READ_ADDR and WRITE_ADDR by a multiple of the transfer size. Specifically, it

increments by transfer size times -1, 0, 1 or 2, depending on the values of CH0_CTRL_TRIG.INCR_READ,

CH0_CTRL_TRIG.INCR_WRITE, CH0_CTRL_TRIG.INCR_READ_REV and CH0_CTRL_TRIG.INCR_WRITE_REV.

The DMA MPU and system-level bus security filters perform protection checks on the lowest byte address of all bytes

transferred on a given cycle (i.e. to the present value of READ_ADDR/WRITE_ADDR). RP2350 memory hardware ensures

unaligned bus accesses do not cause data to be read/written from the other side of a protection boundary. This means

that unaligned access can not be used to violate the memory protection model. Other than this, the result of an

unaligned access is unspecified.

12.6.2.2. Transfer count

Reading TRANS_COUNT (CH0_TRANS_COUNT) returns the number of transfers remaining in the current transfer sequence.

This value updates continuously as the channel progresses. Writing to TRANS_COUNT sets the length of the next transfer

sequence. Up to 228-1 transfers can be performed in one sequence (0x0fffffff, approximately 256 million).

Each time the channel starts a new transfer sequence, the most recent value written to TRANS_COUNT is copied to the live

transfer counter, which will then start to decrement again as the new transfer sequence makes progress. For debugging

purposes, the DBG_TCR (TRANS_COUNT reload value) registers display the last value written to each channel’s TRANS_COUNT.

If the channel is triggered multiple times without intervening writes to TRANS_COUNT, it performs the same number of

transfers each time. For example, when chained to, one channel might load a fixed-size control block into another

channel’s CSRs. TRANS_COUNT would be programmed once by software, and then reload automatically every time.

Alternatively, TRANS_COUNT can be written with a new value before starting each transfer sequence. If TRANS_COUNT is the

channel trigger (see Section 12.6.3.1), the channel will start immediately, and the value just written will be used, not the

value currently in the reload register.

12.6. DMA
1097
